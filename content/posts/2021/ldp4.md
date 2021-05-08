---
title: "炼丹杂记 -- Pytorch view 函数 RuntimeError 问题解决方案"
date: 2021-05-08T18:37:46+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "view", "python"]
draft: false
---

## 问题描述

在程序中使用形如  
```
A = X.view(32, -1)
```
的语句调用 view 时出现如下报错：  
```
RuntimeError: view size is not compatible with input tensor's size and stride (at least one dimension spans across two contiguous subspaces). Use .reshape(...) instead.
```

## 原因分析

view() 需要 Tensor 中元素地址连续，当其不连续时会出现上述问题。  

## 解决办法

出现元素地址不连续情况时，可以先用`.contiguous()`将 Tensor 转化为在内存中连续分布：
```
A = X.contiguous().view(32, -1)
```

问题解决了。  
