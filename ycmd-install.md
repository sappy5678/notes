---
title: ycmd-install
date: 2017-05-27 21:47:33
tags: [emacs,c++,ycmd]
categories: [emacs,ycmd]
---

# 什麼是ycmd
ycmd原本是在vim上的You-Complete-Me, 而後作者把補全部分單獨分離出來做為一個後端,使其他編輯器也能享受這種神級的補全系統.
我目前主要是用它來補全c++,但是他還能夠補全像是go,JS之類的語言,可說是十分全能

<!-- more -->
# 安裝
## 安裝環境 
我的環境是spacemacs + ubuntu 17.04 , 由於spacemacs已經有人幫忙把常用的包都設定好了,所以設定起來可說是相當簡單
## 安裝
1. 首先,先安裝clang,因為這是ycmd的必要套件,這部分我們參考官網的[安裝說明](https://github.com/Valloric/ycmd#building)
2. 在dotfile(快捷鍵Space f e d)中, 按照[spacemacs ycmd layer install](https://github.com/syl20bnr/spacemacs/tree/master/layers/%2Btools/ycmd#ycmd)安裝
3. 測試收工
## 常見問題
* 如果出現這樣的錯誤訊息 
```
layout: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by layout)
```
那麼請先[更新你的函式庫](https://askubuntu.com/a/582910)
如果你的 python 環境是 anaconda, 那麼[這樣更新](https://askubuntu.com/a/786944)
* 路徑不正確  - 他的路徑是吃絕對路徑,所以要像是這樣子
```
  ;;; ycmd 的路徑
  (set-variable 'ycmd-server-command '("python" "/home/sappy5678/emacs_setting/require_plugin/ycmd/ycmd/"))
  ;;; 把要用的資料夾加入白名單
  (setq ycmd-extra-conf-whitelist '("~/hw/*"))
  ;;; 使用ycmd裡面預設的配置
  (set-variable 'ycmd-global-config "/.emacs.d/layers/+tools/ycmd/global_conf.py")
  ;;; 預設開始起自動補全
  (setq ycmd-force-semantic-completion t)
  (add-hook 'doc-view-mode-hook 'auto-revert-mode 'c++-mode-hook 'ycmd-mode)
```

* 要記得安裝auto-complete 讓自動補權的前端顯示出來
