---
title: "ubuntu 上的中文輸入法設定 Chinese Input Method Setup on Ubuntu"
categories: [輸入法]
tags: [輸入法, rime, ibus]
date: 2020-02-12 14:33:20
---

# 緣由
最近在 Ubuntu 上做開發，便來安裝注音輸入法

# 安裝
此方法應該適用於 Ubuntu 12.10 以上版本（在 19.04 中測試過）


1. 安裝 rime `sudo apt-get install ibus-rime`
2. 安裝 注音 `sudo apt-get install librime-data-terra-pinyin librime-data-bopomofo`
3. terminal 輸入 `ibus-setup` 會跳出 IBus Preferences 視窗
4. 在 IBus Preferences -> Input Method -> Add -> Chinese bopomofo
5. 進入 settings -> Region & Language -> Input Sources -> + -> Chinese (Bopomofo)
6. 重新登入即可


# 參考資料
[RimeWithIBus]](https://github.com/rime/home/wiki/RimeWithIBus)
[plum](https://github.com/rime/plum)
[rime-bopomofo](https://github.com/rime/rime-bopomofo)
