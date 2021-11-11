---
title: "社群網路筆記 Networks in Their Surrounding Contexts"
categories: [課程, 社群網路, 清華]
tags: [課程, 社群網路]
date: 2020-04-24 20:23:30
---

考慮 node 的 meta data，這會影響網路的演變
# Homophily - 相似度
相似度越高，越容易相處在一起  

要怎麼知道這個因素是否影響圖的連結  
用 Homophily test  
## Homophily test
跟純 random 比較，看看這因素是否會影響

假設男性占 p 成，女性佔 q 成 (q = 1-p) 
一個 edge 兩端都是男性的機率是 $p^2$，都是女性是 $q^2$ ，一男一女是 $2pq$  
如果跨性別的 edge 顯著比 2pq 還少，那就說明是 homophily  
如下圖  
![homophily_test.png](https://images.sappy.tw/Social_Network/network_with_context/homophily_test.png)  

# homophily 底下的機制 - selection and social influence
* selection - 人會選擇有像似特質的做朋友
* characteristics - 特質
  * immutable - 不可變，人種 
  * mutable - 可變，興趣
* Socialization - 同化現象

## 朋友相似，是因認識前就相似，還是認識後被同化
要觀察網路多個時間  
longitudinal study - 長時間的研究  

假設同個特徵的是朋友，那可能是
1. 因這個特徵變朋友
2. 變朋友後被同化
3. 因其他原因變成朋友，但剛好有同樣特徵

# Affiliation
把 surrounding context (node 的 meta data) 變成網路內部看待  
把 meta data 變成 node，稱為 focal point(或是 foci) ，這稱為 affiliation network  
![affiliation](https://images.sappy.tw/Social_Network/network_with_context/affiliation.png)  

## social-affiliation network
![social-affiliation](https://images.sappy.tw/Social_Network/network_with_context/social-affiliation.png)  

擴展之前說的 triadic closure，讓他也能在 foci 跟 people 上使用  
![extend_triadic_closure](https://images.sappy.tw/Social_Network/network_with_context/extend_triadic_closure.PNG)  



