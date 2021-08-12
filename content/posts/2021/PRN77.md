---
title: "[论文阅读笔记 -- ViT] CAT: Cross Attention in Vision Transformer (2021)"
date: 2021-08-12T16:00:40+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "CNN"]
draft: false
---

# 2106.05786 CAT: Cross Attention in Vision Transformer (2021)

[开源代码传送门](https://github.com/linhezheng19/CAT)

![Fig 1](/images/2021/PRN77/1.png)

## 核心

设计了一种在单通道特征图上做注意力的方法，提出交叉注意力。  

结合 Transformer 和 CNN 的优点构建 CAT。  

![Fig 2](/images/2021/PRN77/2.png)

## Patch 内自注意力块 (IPSA) 与 Patch 间自注意力块 (CPSA)

![Fig 3](/images/2021/PRN77/3.png)

## CAT

![Tab 1](/images/2021/PRN77/T1.png)
