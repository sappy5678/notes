---
title: gdb簡易教學
tags: [gdb,教學]
categories: [程式語言,工具,gdb]
date: 2017-04-08 17:12:01
---

# 簡介
gdb 是一種除錯的工具,經常在 Linux 環境中被使用

<!-- more -->

# 參考資料
* [Linux 除錯利器 - GDB 簡介](http://tetralet.luna.com.tw/?op=ViewArticle&articleId=187&blogId=1)
* [Debugging with GDB （入門篇）](http://www.study-area.org/goldencat/debug.htm)

## 小技巧
### 印出ascii或客製化輸出
* [輸出格式](https://sourceware.org/gdb/onlinedocs/gdb/Output-Formats.html)
* 使用方法
  * EX: p /c char_string
  
### 中斷點條件
[by StackOverFlow](http://stackoverflow.com/questions/4183871/how-do-i-set-a-conditional-breakpoint-in-gdb-when-char-x-points-to-a-string-wh) 
```
break x:20 if strcmp(y, "hello") == 0
```
20 是指行數, x 是可以是任何檔案名稱, 而 y 可以是任何變數
