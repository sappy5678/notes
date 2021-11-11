---
title: "R RStudio 安裝教學"
date: 2017-06-28 00:22:33
tags: [R,RStudio,教學]
categories: [程式語言,R]
---
# 起因
因為之前在 Ubuntu 上安裝 RStudio 時遇到一點問題，便把過程記錄下來，並讓其他人參考

# 安裝過程
> (2017 / 06 /27  安裝)

# 環境需求
* 硬體需求
    * 至少 1GB RAM

* 軟體需求
    * Ubuntu 17.04
    * 要有root or sudo 的權限
<!-- more -->

# 第一步 - 安裝 R
因為 Ubuntu 預設的 R 通常版本都很舊，所以要用到外部別人包好的 R 套件
這裡使用 CRAN 維護的版本 

<font color="red">**注意!! 請選擇可信賴的外部團體維護的版本**</font>


輸入下列指令
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
```

你將會看到如下的輸出
![](https://i.imgur.com/MxCrJIr.png)

再來把套件來源加入到套件管理系統的列表中 
<font color="red">**請把最後的zesty換成你ubuntu的版本**</font>
請參考[版本名稱對照表](https://wiki.ubuntu.com/Releases)

```
sudo add-apt-repository 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu zesty/'
```

更新你的列表資訊


```
sudo apt-get update
```

如果成功你可能會看到如下輸出(get 後的數字不一定會一樣)
![](https://i.imgur.com/C6jkVzl.png)

最後就可以安裝R 了

```
sudo apt-get install r-base
```

如果一切順利，沒有跑出錯誤的訊息那應該就代表你成功了(灑花花

# 第二步 - 安裝 Rstudio
他有分成兩個版本
* Server 版
* Desktop 版
我只有安裝 Desktop版，所以Server板我不清楚怎麼安裝
所以要安裝Server版的請看

[Server 版安裝方式](https://www.rstudio.com/products/rstudio/download-server/)

Desktop 版安裝方式
1. 先去 Rstudio 的官網把 Rstudio 的安裝套件抓下來
	a. [快速連結](https://www.rstudio.com/products/rstudio/download/)
2. 找到你要的版本並下載
    a. ![](https://i.imgur.com/fDVGiIx.png)
    
    b. 看你是 32 bit or 64 bit
    c. Command Line (小黑窗) 底下可以用底下的命令
    
    ```
    wget 下載網址 
    ```
    
3. 因為 RStudio 的新版本加入了兩個相依套件，所以要先安裝
    a. 到 https://packages.debian.org/jessie/amd64/libgstreamer-plugins-base0.10-0/download ， 下載 libgstreamer-plugins-base0.10-0_0.10.36-2_amd64.deb
    
    b. 到  https://packages.debian.org/jessie/amd64/libgstreamer0.10-0/download 下載 libgstreamer0.10-0_0.10.36-1.5_amd64.deb
    
    c.使用 sudo dpkg -i  剛剛下載的兩個套件名稱 安裝套件，大概長這樣 並補充一下，用tab鍵可以自動補全名稱，但是要在同一個目錄底下
    
    ```			
    sudo dpkg -i  libgstreamer-plugins-base0.10-0_0.10.36-2_amd64.deb libgstreamer0.10-0_0.10.36-1.5
    ```
    d. 安裝另外兩個相依套件
    
    ```
    sudo apt install libjpeg62 libedit2
    ```
    e. 最後 安裝 Rstudio
    
    ```
    sudo dpkg -i rstudio-1.0.143-amd64.deb
    ```
    ![](https://i.imgur.com/TTSfnFF.png)
    
到這裡就大功告成啦XDD
