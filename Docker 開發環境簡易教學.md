---
tags: [dev, docker]
title: Docker 開發環境簡易教學
created: '2019-12-28T13:56:20.279Z'
modified: '2019-12-28T13:59:47.803Z'
---

# Docker 開發環境簡易教學
最近在嘗試使用 docker 作為開發環境的選項，這篇只是單純紀錄可能遇到的問題跟常用用法

## 常駐 docker 並能連接上 terminal
用 {} 包起來代表變數
```bash
docker run -id {image}

docker exec -it {id} bash

```
