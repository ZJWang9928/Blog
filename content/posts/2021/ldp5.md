---
title: "炼丹杂记 -- ValueError: only one element tensors can be converted to Python scalars 问题解决方案"
date: 2021-06-24T00:01:18+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "numpy", "tensor", "python"]
draft: false
---

## 问题描述

形如以下操作：  

```
lst = []
a, b = torch.tensor([1, 2, 3]), torch.tensor([4, 5, 6])
lst.append(a)
lst.append(b)
converted_lst = torch.tensor(lst)
```

得到如下报错信息：  

```
ValueError: only one element tensors can be converted to Python scalars
```

## 原因分析

元素为 tensor 的 list 无法转化为 tensor。  

## 解决办法

将 list 中的 tensor 转化为 list：  

```
lst = []
a, b = torch.tensor([1, 2, 3]), torch.tensor([4, 5, 6])
lst.append(a.tolist())
lst.append(b.tolist())
converted_lst = torch.tensor(lst)
```

问题解决了。  
