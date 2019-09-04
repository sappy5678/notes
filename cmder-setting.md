---
title: "cmder 設定"
categories: [tools,命令列,cmder]
tags: [cmder,tools,命令列,設定]
date: 2017-08-20 14:33:20
---

# 緣由
因為最近開始在找 windows 10 系統上好用的另列命令列工具, 在友人的推薦下使用了這個

# 安裝
由於他的安裝只是把它解壓縮而已, 在此就不多說了

# 設定
## ls 中文資料夾亂碼
1. 打開Settings (快速鍵 win+ctrl+p)
2. 在Settings > Startup > Environment裡加入：set LANG=zh_TW.UTF8
3. 重開

## 更改初始資料夾
1. 切到 setting -> Startup -> Tasks -> {cmd}
2. 最後面加入(D:\Data 可以換成你想要的路徑)
```
-new_console:d:"D:\Data"
```
# 參考資料
[子風的知識庫](http://zwindr.blogspot.tw/2016/01/cmder-setting-1.html)

