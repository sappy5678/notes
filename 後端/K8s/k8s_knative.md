---
title: "Kubernetes 筆記 - Knative"
tags: [Kubernetes, serverless]
categories: [Kubernetes]
created: '2020-05-02T11:56:20.279Z'
modified: '2020-05-02T13:55:38.356Z'
---

# kubespray
按照上次安裝
# istio
## 安裝 istioctl 
https://istio.io/docs/setup/getting-started/
## 安裝 istio
`istioctl install`
https://istio.io/docs/setup/install/istioctl/

# knative
# metalLB
## 安裝 
https://metallb.universe.tf/installation/

## 設定 Layer 2 (網路架構的L2)
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.1.240-192.168.1.250 # 多 ip
      - 192.168.1.240/32  # 單 ip
      
```

當設定成功後輸入 `kubectl get service -n istio-system istio-ingressgateway`，便能看到EXTERNAL-IP從 PENDING 變成有 ip