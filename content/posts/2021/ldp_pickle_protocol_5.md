---
title: "炼丹杂记 -- Python 读取 pkl 文件报错 ValueError: unsupported pickle protocol: 5 解决办法"
date: 2021-09-26T21:00:54+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python", "protocol", "linux", "windows", "pickle"]
draft: false
---

## 问题描述

使用 Python 读取 .pkl 文件时报错如下：  

```
ValueError: unsupported pickle protocol: 5
```

## 原因分析

Python 3.8 以上版本在保存 .pkl 文件时使用的协议号为 5，即 protocol 关键字为 5，若在读取时使用低于 3.8 版本的 Python，由于这个协议号不向下兼容，导致报错。  

## 解决办法

使用均高于或均低于 3.8 版本的 Python进行保存和读取即可。  
