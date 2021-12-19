---
title: "炼丹杂记 -- Python 报错 RuntimeError: ... is at version 2; expected version 1 ... 的一种解决办法"
date: 2021-12-12T17:35:03+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python", "pytorch", "linux", "cuda"]
draft: false
---

## 问题描述

模型训练时遇到如下报错：  

RuntimeError: one of the variables needed for gradient computation has been modified by an inplace operation: [torch.FloatTensor [50, 76, 512]] is at version 2; expected version 1 instead. Hint: enable anomaly detection to find the operation that failed to compute its gradient, with torch.autograd.set_detect_anomaly(True).

## 一种解决办法

将原先的 `loss.backward()` 改为  

```
loss1 = loss.detach_().requires_grad_(True)
loss1.backward()
```

问题解决了。  
