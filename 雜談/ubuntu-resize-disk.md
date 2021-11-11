---
title: "Ubuntu 調整硬碟大小教學"
date: 2017-09-14 21:13:33
tags: [Ubuntu,硬碟,教學,調整,Gparted]
categories: [工具,Ubuntu,硬碟,Gparted]
---
# 前言
最近在安裝 opencv 時，因為VM硬碟大小不夠，所以只好擴充硬碟大小，但是 root partition 好像沒辦法在開機時調整大小(不確定)，所以這次用 Gparted

# 過程
首先，先用 live cd 做開機
[參考這篇](http://xx3d2ybnf.pixnet.net/blog/post/128713507-vmware%25E8%2599%259B%25E6%2593%25AC%25E6%25A9%259F%25E5%2599%25A8%25E9%2580%25B2%25E5%2585%25A5bios%25E6%2588%2596%25E6%2598%25AF%25E9%2596%258B%25E6%25A9%259F%25E9%2581%25B8%25E5%2596%25AEboot-menu%25E7%259A%2584)

然後打開 Gparted
會看到被紅色圈起來的部分，然後對她右鍵，按下 swapoff
![snipaste_20170914_211549](images/snipaste_20170914_211549.png)
然後把他移動到右邊(先擴充到右邊，移過去，再縮小)，並且在對他右鍵做swapon 就完成了(灑花花