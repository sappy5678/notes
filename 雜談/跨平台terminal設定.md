---
title: "跨平台 terminal 設定"
categories: [工具]
tags: [工具]
date: 2020-03-28 23:49:20
---

# 緣由
最近在因為 terminal 跨平台讓我有點煩躁，所以才寫下這篇備忘

# 安裝
目前有兩款比較熱門的跨平台 terminal
1. [hyper](https://hyper.is/)
2. [terminus](https://eugeny.github.io/terminus/)

就顏質跟設定方便度而言，我選擇了後者，也就是 [terminus](https://eugeny.github.io/terminus/)

## Windows 安裝
1. 使用 [chocolatey](https://chocolatey.org/) 安裝
   `choco install terminus`
2. 安裝 [powershell core](https://github.com/PowerShell/PowerShell)，
   `choco install powershell-core`
3. 安裝 [nerd fonts](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts) 中任意喜歡的字體
   我個人比較推 `FiraCode` `SourceCodePro` `CascadiaCode` 這三款
4. 打開 terminus，在 terminus 的設定 -> 外觀 -> 字體上選擇字體

