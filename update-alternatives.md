---
title: "update-alternatives 多版本控制"
categories: [ubuntu,tools,update-alternatives]
tags: [ubuntu,tools,update-alternatives,多版本控制]
date: 2017-10-09 18:14:32
---

# 起因
因為要安裝 cuda ,但是我的 gcc/g++ 版本已經到了6.3了,因此得想辦法降回 4.x

# 解法
[參考](https://askubuntu.com/a/26518/745445)
注意: 優先度（install 最後的數字）是按照 大到小的

[更詳細的參數](http://blog.csdn.net/jasonding1354/article/details/50470109)

可以用 --query 來找有沒有替代板