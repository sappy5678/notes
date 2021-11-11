---
title: "protobuf install"
categories: [工具,protobuf,安裝]
tags: [工具,protobuf,安裝]
date: 2017-10-14 09:37:58
---

# 前言
最近在弄 caffe 的安裝，過程中需要安裝到 opencv，但opencv又需要安裝到 protobuf

# 環境
* ubuntu 17.04

# 過程
基本上按照官網做就好了，唯一需要注意的是，如果有安裝 anconda ，可能會吃到anaconda內建的libtool，因此我的作法是更新anaconda的libtool，讓它版本相符合
