---
title: "炼丹杂记 -- Pytorch 取矩阵对角线元素"
date: 2021-10-12T19:13:06+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python", "pytorch", "linux", "matrix"]
draft: false
---

在 Pytorch 中，可以用 torch.diag 取矩阵的对角线元素，返回的是一个向量；使用 torch.diag_embed 可将向量变换为以该向量为对角线的方阵。  

```
In [1]: import torch
In [2]: A = torch.randn(3, 3)
In [3]: A
Out[3]:
tensor([[ 0.1808,  0.2905, -2.2199],
[-0.8913,  1.1296,  0.6261],
[-1.3064,  0.9235, -1.0164]])
In [4]: diag = torch.diag(A)
In [5]: diag
Out[5]: tensor([ 0.1808,  1.1296, -1.0164])
In [6]: a_diag = torch.diag_embed(diag)
In [7]: a_diag
Out[7]:
tensor([[ 0.1808,  0.0000,  0.0000],
[ 0.0000,  1.1296,  0.0000],
[ 0.0000,  0.0000, -1.0164]])
```
