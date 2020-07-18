---
tags: [dev, ElasticSearch, 中文, NLP]
categories: [ElasticSearch]
title: Elasticsearch 中文斷詞處理

---

# 目的
解決 elasticsearch 中，中文斷詞預設是 character-base 的斷詞方式  
例如 `今天天氣很好` 應該要斷成 `今天 天氣 很 好`，而不是 elasticsearch 中的 `今 天 天 氣 很 好`  

# 解法
中文斷詞目前最好的應該還是中研院的 [CKIPTagger](https://github.com/ckiplab/ckiptagger) ，我們是自行斷完詞之後再用 elastic search 的 space 斷詞塞進去資料庫  
中研院的斷詞器 CKIPtagger，是中研院第二版的斷詞器，主要是用類神經網路做，或者可以跟他們申請第一版的斷詞器，第一版主要是使用傳統斷詞方法  
目前我們是自行撰寫一支 Python 程式來用 CKIPtagger 做斷詞，斷好的結果再放在 Document 中 POST 到 Elasticsearch，選擇這樣做是因為做起來比較靈活，可以自由修改處理過程，也不用擔心改版的時候 plugin 不能用  
為了之後能繼續存取原始資料來做分析或其他用途，我們是存成兩份資料，一份原始資料，一份是斷詞斷完的資料  
大概長得像下面這樣  
```json
{
    "origin": "",
    "token": ""
}
```
