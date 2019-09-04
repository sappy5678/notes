---
title: "Tensorflow with Gpu 安裝教學"
categories: [ML,Tensorflow,教學]
tags: [ML,Tensorflow,教學]
date: 2017-09-28 22:34:55
---

# 起因

因為教授要求要做物體辨識, 加上實驗室的Server目前暫時不能使用,所以只好先在自己的電腦上面跑

# 環境 

* Ubuntu 17.04
* Tensorflow 1.3
* Cuda 9.0
* Cudnn 7.0 

<!--more-->

# 過程

1. 安裝 nvidia 的驅動程式

2. 安裝 cuda - 從官方網站上的 deb 包安裝即可

3. cudnn 要手動安裝,所以先下載 tar 檔案,

   進到放著tar的資料夾

   用 下面指令解壓縮

   ```shell
   tar -xzvf cudnn-9.0-linux-x64-v7.tgz
   ```

   複製到目錄

   ```shell
   $ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
   $ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
   $ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
   ```

   ​

4. 到 `/usr/local/cuda/lib64` 底下

   使用下面指令, 創造一個軟連結 （因為 Tensorflow 使用 cudnn 6.0, 所以要用這方法把 讓它可以使用 libcudnn.so.6 這個檔案）

   ```shell
   ln -s libcudnn.so.7.* libcudnn.so.6
   ```

5. 用 pip 安裝 tensorflow with Gpu

6. 在 `~/.bashrc` 最底下加上

   ```shell
   export CUDA_HOME=/usr/local/cuda/
   export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
   ```

## 選用

如要使用 pycharm 要記得幫它加上環境變數