---
title: "[论文阅读笔记 -- ViT] Early Convolutions Help Transformers See Better (2021)"
date: 2021-07-27T11:07:09+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "CNN"]
draft: false
---

# 2106.14881 Early Convolutions Help Transformers See Better (2021)

![Fig 1](/images/2021/PRN60/1.png)

ViT 模型对超参数敏感，不好优化；而 CNN 更容易优化。  

本文认为问题在于 ViT 的前期视觉处理 (early visual processing)，通常是 patchify stem，实现为步长为 \\(p\\) 的 \\(p \times p\\) 卷积，\\(p\\) 通常默认取 16，是大步长大卷积核的卷积操作。  

文章将 patchify 操作替换成标准卷积 (多个步长为 2 的 \\(3 \times 3\\) 卷积) 进行实验对比，发现替换后各方面表现更优。  
