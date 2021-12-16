---
title: "炼丹杂记 -- PyTorch 报错 Got 3 and 1 in dimension 1 的一种解决办法"
date: 2021-12-16T17:34:51+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "coding", "python"]
draft: false
---

# 问题描述

在 dataloader 相关部分出现如下报错：  

```
RuntimeError: invalid argument 0: Sizes of tensors must match except in dimension 0. 
Got 3 and 1 in dimension 1 at /pytorch/aten/src/TH/generic/THTensorMath.c:3586
```

# 原因分析

RGB 图像中混有单通道的灰度图，导致堆叠时无法对齐。  

# 解决办法

读取图像后进行检查与转化：  

```
image = Image.open(image_path)
if image.getbands()[0] == 'L':
     image = image.convert('RGB')
```

问题解决了。  
