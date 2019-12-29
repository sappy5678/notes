---
tags: [dev, docker]
title: Docker 開發環境簡易教學
created: '2019-12-28T13:56:20.279Z'
modified: '2019-12-29T11:55:38.356Z'
---

# 簡介
最近在嘗試使用 docker 作為開發環境的選項，這篇只是單純紀錄可能遇到的問題跟常用用法

## 常駐 docker 並能連接上 terminal
用 {} 包起來代表變數
```bash
docker run -id {image}

docker exec -it {id} bash
```

## 做 fuzz test
```bash
docker run -id --ulimit core=-1 --privileged {image}
```
[參考](https://ephrain.net/docker-%E5%9C%A8-container-%E8%A3%A1%E8%A8%AD%E5%AE%9A-core-dump-%E7%9A%84%E6%AA%94%E6%A1%88%E5%90%8D%E7%A8%B1%E6%A0%BC%E5%BC%8F/)

## 打包現有環境到新 image
```bash
docker commit {old_running_docker} {newimagename}
```

[參考](https://stackoverflow.com/questions/28302178/how-can-i-add-a-volume-to-an-existing-docker-container)
