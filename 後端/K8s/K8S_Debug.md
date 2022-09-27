---
title: K8S debug 技巧
tags:
  - Kubernetes
categories:
  - Kubernetes
date: 2022-09-27T16:00:33.400Z
---

紀錄一下最近找問題時用到的指令

## 查詢上一個 pod 的 log
發現 pod 有重啟的紀錄，因此透過這指令去查死掉的 pod 的 logs 來得知發生甚麼事情
```
kubectl logs -f -n {NAMESPACE} pod/{PODNAME} --previous
```