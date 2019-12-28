---
tags: [pyspark, python, spark]
title: pyspark 小技巧
created: '2019-12-28T14:00:56.517Z'
modified: '2019-12-28T14:06:02.556Z'
---

# pyspark 小技巧
最近在弄 spark 時遇到一堆破事，特此紀錄

## collect 很久，甚至沒反應

``` python
conf = SparkConf().setMaster("local[*]") \
    .setAppName("App_Name") \
    .set('spark.executor.memory', '4G') \
    .set('spark.driver.memory', '45G') \
    .set('spark.driver.maxResultSize', '10G')
self.sc = SparkContext(conf=conf)
```

## saveAsFile 很久，甚至沒反應

``` python
conf = SparkConf().setMaster("local[*]") \
    .setAppName("App_Name") \
    .set('spark.executor.memory', '4G') \
    .set('spark.driver.memory', '45G') \
    .set('spark.driver.maxResultSize', '10G')
self.sc = SparkContext(conf=conf)
```

## for loop 越跑越慢
使用 repartition() 或 checkpoint() 可以解決這問題
https://changhsinlee.com/pyspark-dataframe-basics/

