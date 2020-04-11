---
title: "用 QEMU 建立VM"
categories: [雲端計算,VM,QEMU]
tags: [雲端計算,VM,QEMU]
date: 2020-03-29 23:49:20
---

# 緣由
練習 QEMU

# 安裝
目前有兩款比較熱門的跨平台 terminal
1. [hyper](https://hyper.is/)
2. [terminus](https://eugeny.github.io/terminus/)

就顏質跟設定方便度而言，我選擇了後者，也就是 [terminus](https://eugeny.github.io/terminus/)

## 建立VM並開機
1. 使用 [chocolatey](https://chocolatey.org/) 安裝
   `choco install terminus`
2. 安裝 [powershell core](https://github.com/PowerShell/PowerShell)，
   `choco install powershell-core`
3. 安裝 [nerd fonts](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts) 中任意喜歡的字體
   我個人比較推 `FiraCode` `SourceCodePro` `CascadiaCode` 這三款
4. 打開 terminus，在 terminus 的設定 -> 外觀 -> 字體上選擇字體


開機
```bash
qemu-img create -f qcow2 1.qcow2 40G

qemu-system-x86_64 --enable-kvm -hda 1.qcow2 -m 1024 -net nic,model=virtio -net user,hostfwd=tcp::2223-:22 -cdrom ../ubuntu-16.04.6-desktop-amd64.iso -vga std -cpu host -smp 4,cores=4,threads=1,sockets=4,maxcpus=16 -boot strict=on
```

必要安裝
```bash
sudo apt install openssh-server

sudo sed -i 's/APT::Periodic::Update-Package-Lists "1"/APT::Periodic::Update-Package-Lists "0"/' /etc/apt/apt.conf.d/20auto-upgrades

```

安裝 tensorflow
--no-cache-dir tensorflow 是為了修正 memory error 的 bug
```
pip3 install --no-cache-dir tensorflow
```

編譯 kernel
```bash
uname -r

mkdir kernel
cd kernel

wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.6.2.tar.xz

tar Jxvf linux-5.6.2.tar.xz

sudo mv linux-5.6.2/ /usr/src/

sudo apt install fakeroot build-essential libncurses5-dev kernel-package openssl bison flex libssl-dev

cd /usr/src/linux-5.6.2

# sudo cp /boot/config-`uname -r` ./.config

# make oldconfig

touch REPORTING-BUGS

make menuconfig

sudo fakeroot make-kpkg -j4 --initrd kernel_image kernel_headers
sudo fakeroot make-kpkg -j32 --initrd kernel_image kernel_headers

cd ..

wget 192.168.122.1:8000/linux-image-5.6.2_5.6.2-10.00.Custom_amd64.deb
wget 192.168.122.1:8000/linux-headers-5.6.2_5.6.2-10.00.Custom_amd64.deb

sudo dpkg -i linux-image-5.6.2_5.6.2-10.00.Custom_amd64.deb

sudo dpkg -i linux-headers-5.6.2_5.6.2-10.00.Custom_amd64.deb

sudo reboot now

uname -r

systemd-analyze 

```

compiler
```bash
sudo apt install software-properties-common
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install gcc-9 g++-9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90 --slave /usr/bin/g++ g++ /usr/bin/g++-9 --slave /usr/bin/gcov gcov /usr/bin/gcov-9
```

分析
systemd-analyze blame

# Todo
* [] Compiler 優化
* 