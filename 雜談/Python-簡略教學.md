---
title: Python 簡略教學
date: 2017-03-05 10:39:58
tags: [Python,教學]
categories: [程式語言,Python]
---

# 安裝
  1. 請先到 [Anaconda](https://www.continuum.io/downloads) 安裝Anaconda (建議 Python 3.x 版本)
  2. 再來到 [Pycharm](https://www.jetbrains.com/pycharm/download/#section=windows) 安裝 IDE (Community 版本是免費的)
  3. 安裝完成後, 打開 Pycharm 新增一個 .py 檔, 並輸入 print("Hello World"), 之後執行看看, 看看有沒有印出 Hello World

<!-- more -->
# 基本語法
## 使用方式
  * 我會盡量為每個部份都寫一些教學以及練習題, 讓大家可以更快速的上手
  * [練習題](https://repl.it/classroom/invite/DacYfXQ) 如果想要練習的人可以進來[這邊](https://repl.it/classroom/invite/DacYfXQ), 裡面有一些基礎題讓大家練習
## 基本輸入輸出
### 輸出
  Python 是個很口語化的程式語言
  如同你剛才所看到的 
  ```python=
  print("Hello World")
  ```
  便代表著印出 Hello World 這幾個單字, 當然, 中文也是OK的, 只要你將他包裹在 "" 兩個引號之中
  便可以將他印出來, 現在換你來試試看
  <iframe frameborder="0" width="100%" height="500px" src="https://repl.it/GHhL/1?lite=true"></iframe>
  [練習](https://repl.it/student/submissions/587564)
  
### 輸入
  至於要如何輸入呢?
  Python 也提供了很口語化的方法 " input() "
  像是這樣
  <iframe frameborder="0" width="100%" height="1000px" src="https://repl.it/GHhL/5?lite=true"></iframe>
  [練習](https://repl.it/student/submissions/587588)

## 資料型態
  但首先, 我們要認識一下變數
  變數, 便是我們之前說過的, 給某些東西一個名子
  也可以想成是數學中的 x = 1 這種代數
  而資料型態則是告訴 Python 你那個東西是什麼, 例如 數字或是字串
  Python 常見以下幾種資料型態
  * int (整數): 例如 0 1 2 .... , 也可以是負數, 像是 -598 -753 -1 之類的
  * float (浮點數): 也是我們所謂的小數點, 例如 3.1415926 123.999 -3.784 等等,要注意的是, 小數點會有誤差, 所以使用時要特別小心 
  * string (字串): 例如我們一開始的 "Hello World" 或者是 "Input" 等等, 這裡要注意的是,字串都要用 "" 兩個引號包起來, 而且你所有的 input() 的值, 都會被當成字串看待
  * bool (布林): 他只有兩個值, True 和 False, 常用來作為條件判斷用
  * list (列表): 用來同時存放很多資料的型態, 類似於數學上的矩陣,之後會有更詳細的介紹
  * tuple (元組): 跟 list 類似, 最大的不同在於這個是不能改變的, 之後會有更詳細的介紹
  * dict (字典): 存放很多資料的型態,常用於做出類似層級的概念, 之後會有更詳細的介紹
  
### int (整數), float (浮點數)
  廢話不多說, 直接進入正題
  <iframe frameborder="0" width="100%" height="1000px" src="https://repl.it/GHhL/8?lite=true"></iframe>
  [練習](https://repl.it/student/submissions/597180)
  
### list (列表), tuple (元組)
  <iframe frameborder="0" width="100%" height="500px" src="https://repl.it/GHhL/13?lite=true"></iframe>

### dict (字典)
  <iframe frameborder="0" width="100%" height="500px" src="https://repl.it/GHhL/17?lite=true"></iframe>

## 流程控制
  我們已經學會了那麼多的型態, 然而, 當情況變得更複雜時, 我們可能會遇到一些需要判斷的問題.  例如, 當我們要判斷考試的成績是否及格時, 要怎麼做呢?
  所以我們接下來就要學到如何判斷.
  希望大家還記得我們之前說道的 bool 嗎? 他只有 True 跟 False 兩種型態, 接下來我們就會使用他來做判斷
  一些基本的條件判斷:
    * >= 大於等於
    * >  大於
    * == 等於
    * <= 小於等於
    * <  小於
    * != 不等於
    
### if elif else 
  * if - 如果, 你可以把他想像成如果條件為真的話, 要做些甚麼
  * elif - 當第一個條件不符合時, 就會來檢查這裡的條件, elif 是 else if 的縮寫
  * else - 其他, 就是當上面所有條件都不符合時, 就會來執行這裡面的動作
  接下來是例行的範例時間XDD
  <iframe frameborder="0" width="100%" height="700px" src="https://repl.it/GHhL/18?lite=true"></iframe>
  [練習](https://repl.it/student/submissions/598347)

### for while
  如果我們現在要印出 1 ~ 100 要怎麼辦呢?
  如果只能夠 一個個慢慢印出來那不是要印 100 次嗎?
  所以便出現了迴圈, 能夠讓我們執行一些步驟很多次
  所以這樣就不需要打 100 次了
  直接上範例 XDD
  <iframe frameborder="0" width="100%" height="700px" src="https://repl.it/GHhL/19?lite=true"></iframe>
  [練習](https://repl.it/student/submissions/598505) 

## function

# library
## 安裝

## 使用
