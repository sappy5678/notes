---
title: 使用 Pulumi 自動建立 Proxmox LXC 環境
date: 2024-01-14T15:08:46.076Z
preview: ""
draft: false
tags:
    - HomeLab
    - Pulumi
    - Proxmox
categories:
    - HomeLab
created: 2024-01-14T15:07:41.280Z
modified: 2024-01-14T15:18:00.351Z
---
# 使用 Pulumi 自動建立 Proxmox LXC 環境

# 簡介

最近在嘗試使用 Pulumi + ProxmoxVE 搭建 HomeLab 的開發環境 

摸了一段時間後終於搞清楚怎麼用了，因此這邊簡單做個紀錄

我們這次使用的是 [https://github.com/muhlba91/pulumi-proxmoxve](https://github.com/muhlba91/pulumi-proxmoxve) 這個第三方的元件，他是透過  tf-bridge  再去使用 [https://github.com/bpg/terraform-provider-proxmox](https://github.com/bpg/terraform-provider-proxmox) 這個 terraform 的元件

# 安裝步驟

## 建立 Pulumi 專案

首先，我們先建立一個空資料夾作為專案的資料夾

並在資料夾中執行 `pulumi new python` 來建立一個新專案

我們會看到，資料夾的結構變成這樣

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled.png)

`Pulumi.yaml` 裡面出現的會是我們剛剛填入的資料，其中 `options` 可以指定 python 環境要不要由 Pulumi 控制，預設會在專案內建立 `venv` 這個資料夾，並使用該環境來執行 pulumi

如果想要由自己來管理環境的話，可以把這個選項拿掉，他就會用目前 shell 環境下的 python 去執行

```yaml
name: homelab
runtime:
  name: python
  options:
    virtualenv: venv
description: a homelab iac
```

## 安裝套件

接下來就是安裝我們需要的元件，我們根據 [說明](https://www.pulumi.com/registry/packages/proxmoxve/installation-configuration/)，安裝對應語言的函式庫

這邊以 python 為例子，指令如下 `pip install pulumi-proxmoxve`

## 設定 provider

安裝完成後，就可以開始來設定我們的 provider 了

provider 需要的參數有以下這些

- resource_name - Pulumi 的辨識名稱，跟 Proxmox 無關，填一個好記的字串即可
- insecure - 是否跳過 TLS 檢查，這邊我選是
- endpoint - Proxmox 的位置，填入 IP Port 就好了
- api_token - 呼叫API時，用來驗證使用者身分的工具，可以在 datacenter 底下建立，建立完成後他會給你一個 UUID，再把 User 、 TokenID、剛剛拿到的 UUID 填入下面的程式即可
    
    ![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%201.png)
    

```python
import pulumi
import pulumi_proxmoxve as pve

provider = pve.Provider(
    resource_name="{Name}",
    insecure=True,
    endpoint="https://{IP}:{Port}/",
    api_token="{User}!{TokenID}={UUID}",
)
```

## 給 APIToken 權限

接下來我們需要給 APIToken 相對應的權限，這邊我是給 PVEAdmin

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%202.png)

## 給 LXC 名稱 + ID，以及登入用的 ssh key

接下來看看 LXC 的每個步驟分別需要那些基本參數

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%203.png)

```python
import pulumi
import pulumi_proxmoxve as pve
keys = ["{ssh public key}"] # 我是使用 ssh 登入，所以把 ssh publisc key 放這邊

pulumiTestCT = pve.ct.Container(
    resource_name="{Name}", 
    opts=pulumi.ResourceOptions(provider=provider), # 跟程式說使用我們之前設定的 provider 來執行操作
    vm_id={ID}, # CT ID
    unprivileged=True, # 對應到 unprivileged container
    started=True, # 設定完成後自動啟動
    start_on_boot=True, # 開機後自動啟動
    node_name="{NodeName}", # 對應到 node 那個欄位
    initialization=pve.ct.ContainerInitializationArgs(
        hostname="{Name}", # 對應到 Hostname 那個欄位
        user_account=pve.ct.ContainerInitializationUserAccountArgs(keys=keys),  # 我是使用 ssh 登入，所以把 ssh publisc key 放這邊
    ),
    features=pve.ct.ContainerFeaturesArgs(nesting=True) # 對應到 Nesting 
)
```

## 找到需要的 CT Template

可以使用 `pveam list local` 這個指令找出 CT template 的位置 

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%204.png)

```python
pulumiTestCT = pve.ct.Container(
    resource_name="{Name}",
    opts=pulumi.ResourceOptions(provider=provider),
    vm_id={ID},
    unprivileged=True,
    started=True,
    start_on_boot=True,
    node_name="{NodeName}",
    features=pve.ct.ContainerFeaturesArgs(nesting=True),
    operating_system=pve.ct.ContainerOperatingSystemArgs( # 設定我們要的 OS
        template_file_id="local:vztmpl/debian-12-standard_12.2-1_amd64.tar.zst", # 可以使用 pveam list local 這個指令找出 CT template 的位置 
        type="debian", # 不同作業系統
    ),
    initialization=pve.ct.ContainerInitializationArgs(
        hostname="{Name}",
        user_account=pve.ct.ContainerInitializationUserAccountArgs(keys=keys),
    ),

)
```

## 設定開機硬碟

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%205.png)

```python
pulumiTestCT = pve.ct.Container(
    resource_name="{Name}",
    opts=pulumi.ResourceOptions(provider=provider),
    vm_id={ID},
    unprivileged=True,
    started=True,
    start_on_boot=True,
    node_name="{NodeName}",
    features=pve.ct.ContainerFeaturesArgs(nesting=True),
    operating_system=pve.ct.ContainerOperatingSystemArgs(
        template_file_id="local:vztmpl/debian-12-standard_12.2-1_amd64.tar.zst", 
        type="debian",
    ),
    initialization=pve.ct.ContainerInitializationArgs(
        hostname="{Name}",
        user_account=pve.ct.ContainerInitializationUserAccountArgs(keys=keys),
    ),
    disk=pve.ct.ContainerDiskArgs(datastore_id="localHDD"), # 指定要使用哪個儲存空間

)
```

## 設定 CPU/記憶體

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%206.png)

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%207.png)

因為這次只是測試，所以 CPU Memory 都不用填，都使用預設值就好

## 設定網路

![Untitled](https://images.sappy.tw/HomeLab/%E4%BD%BF%E7%94%A8Pulumi%E8%87%AA%E5%8B%95%E5%BB%BA%E7%AB%8BProxmoxLXC%E7%92%B0%E5%A2%83/Untitled%208.png)

```python
pulumiTestCT = pve.ct.Container(
    resource_name="{Name}",
    opts=pulumi.ResourceOptions(provider=provider),
    vm_id={ID},
    unprivileged=True,
    started=True,
    start_on_boot=True,
    node_name="{NodeName}",
    features=pve.ct.ContainerFeaturesArgs(nesting=True),
    operating_system=pve.ct.ContainerOperatingSystemArgs(
        template_file_id="local:vztmpl/debian-12-standard_12.2-1_amd64.tar.zst", 
        type="debian",
    ),
    disk=pve.ct.ContainerDiskArgs(datastore_id="localHDD"),
    initialization=pve.ct.ContainerInitializationArgs(
        hostname="{Name}",
        user_account=pve.ct.ContainerInitializationUserAccountArgs(keys=keys),
        ip_configs=[
            pve.ct.ContainerInitializationIpConfigArgs(
                ipv4=pve.ct.ContainerInitializationIpConfigIpv4Args(
                    address="{IP/CIDR}", gateway="{Gateway}" # 設定 IP/CIDR + gateway
                )
            )
        ],
    ),
    network_interfaces=[
        pve.ct.ContainerNetworkInterfaceArgs(name="eth0", firewall=True) # 使用 eth0，並打開防火牆(也可以不開)
    ]

)
```

## 組合上述步驟 + 部屬

最後我們把所有步驟組合起來，會變成下面這樣，就大功告成了

現在我們可以使用 `pulumi up` 來部屬了

```python
import pulumi
import pulumi_proxmoxve as pve
keys = ["{ssh public key}"] 

provider = pve.Provider(
    resource_name="{Name}",
    insecure=True,
    endpoint="https://{IP}:{Port}/",
    api_token="{User}!{TokenID}={UUID}",
)

pulumiTestCT = pve.ct.Container(
    resource_name="{Name}",
    opts=pulumi.ResourceOptions(provider=provider),
    vm_id={ID},
    unprivileged=True,
    started=True,
    start_on_boot=True,
    node_name="{NodeName}",
    features=pve.ct.ContainerFeaturesArgs(nesting=True),
    operating_system=pve.ct.ContainerOperatingSystemArgs(
        template_file_id="local:vztmpl/debian-12-standard_12.2-1_amd64.tar.zst", # pveam list local
        type="debian",
    ),
		disk=pve.ct.ContainerDiskArgs(datastore_id="localHDD"),
    initialization=pve.ct.ContainerInitializationArgs(
        hostname="{Name}",
        user_account=pve.ct.ContainerInitializationUserAccountArgs(keys=keys),
        ip_configs=[
            pve.ct.ContainerInitializationIpConfigArgs(
                ipv4=pve.ct.ContainerInitializationIpConfigIpv4Args(
                    address="192.168.51.5/24", gateway="192.168.51.1"
                )
            )
        ],
    ),
    network_interfaces=[
        pve.ct.ContainerNetworkInterfaceArgs(name="eth0", firewall=True)
    ]
   
)
```