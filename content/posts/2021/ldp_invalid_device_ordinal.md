---
title: "炼丹杂记 -- Python 报错 RuntimeError: CUDA error: invalid device ordinal 解决办法"
date: 2021-10-13T23:06:19+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python", "pytorch", "linux", "cuda"]
draft: false
---

## 问题描述

在涉及 to(device) 操作时出现如下报错：  

```
RuntimeError: CUDA error: invalid device ordinal
```

## 解决办法

在该操作前指定可选的 GPU 卡：  

```
import os
os.environ['CUDA_VISIBLE_DEVICES'] = "0,1,2,3,4,5,6,7"
```
