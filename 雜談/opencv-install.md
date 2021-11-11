---
title: "OpenCV 安裝教學"
date: 2017-09-14 16:07:33
tags: [Opencv3,Ubuntu,安裝,教學]
categories: [工具,Opencv3]
---
# 前言
因為教授要求安裝 opencv，但是因為 opencv 安裝複雜，所以在此紀錄一下

# 環境要求
* opencv3.3
* ubuntu 17.04

# 安裝紀錄
首先，我是參考 [How to install OpenCV 3.1 on Ubuntu 14.04 64bits](https://gist.github.com/MarcWang/0547f87cf777b6576275)
以及[官方文件](http://docs.opencv.org/trunk/d7/d9f/tutorial_linux_install.html)

一開始我是先 按照 [How to install OpenCV 3.1 on Ubuntu 14.04 64bits](https://gist.github.com/MarcWang/0547f87cf777b6576275) 安裝

在下面這行遇到一些困難
```
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
```
首先，libjasper-dev 已經被從 ubuntu 17 上移除了，所以我是透過 [這個方法](https://stackoverflow.com/a/44488374/4754280) 來安裝 ubuntu 16 版的 libjasper-dev
其次
libpng12-dev 已經被 libpng12-0 所取代了，所以把上面的這部分換掉即可

這裡說明一下比較會用到的編譯的參數( [官方文件](http://docs.opencv.org/trunk/d7/d9f/tutorial_linux_install.html) 上都找的到)
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..
```
* CMAKE_BUILD_TYPE 可以設為 Release or Debug 
* 最後的 .. 是指 opencv 原始碼的路徑
*  OPENCV_EXTRA_MODULES_PATH 要放 opencv_contrib/modules 的路徑


之後參考[這篇](http://cccharles.pixnet.net/blog/post/338054801-%25E5%259C%25A8ubuntu-16.04%25E5%25AE%2589%25E8%25A3%259Dopencv-3.1) 安裝 Anaconda 的 opencv 以及一些加速器
 
最後做完一些測試就OK了 ^^