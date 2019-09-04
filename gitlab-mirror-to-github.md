---
title: "如何在github上建立gitlab的鏡像"
date: 2017-08-15 14:47:27
tags: [gitlab,github,mirror]
categories: [工具,gitlab]
---

# 起因
因為 gitlab 上相對於 github 的可玩性跟福利都比較大所以打算逐漸遷移過去, 然而由於 github 上使用的人數依然比較多, 便想說能不能在 github 上面建立一個 gitlab 的複製版

<!-- more -->

# 設定
1. 先在 github 上新開一個對應的 repo 
2. [產生 token](https://github.com/settings/tokens/new?scopes=repo&description=GitLab+mirror)
3. 先進入這裡 ![gitlab ui](http://i.imgur.com/ukP9eme.png)
4. 打勾, 並且按照格式輸入 ![gitlab ui](http://i.imgur.com/zUVMT7l.png)
5. 按下存檔並且更新(可能要等一段時間), 如果你發現github上的repo有新檔案了(更新完後等 3~5分鐘),那就代表你成功了(灑花花)

# 參考資料
[espadav8 blog](https://espadav8.co.uk/2016/08/22/gitlab-mirror-pushing-to-github/)

