---
title: "[论文阅读笔记 -- ViT] Contextual Transformer Networks for Visual Recognition (CVPRW 2021)"
date: 2021-09-02T21:44:32+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "CNN"]
draft: false
---

# 2107.12292 Contextual Transformer Networks for Visual Recognition (CVPRW 2021)

[开源代码传送门](https://github.com/JDAI-CV/CoTNet)

![Fig 1](/images/2021/PRN89/1.png)

## 概述

### 现有 ViT 方法的问题

只是基于各位置上孤立的 key 和 query 求得注意力矩阵，忽略了相邻 key 之间丰富的上下文信息。  

### 本文提出一个问题

是否存在一种优雅的方式，能够利用 2D 特征图中输入的 key 之间丰富的上下文信息增强 Transformer 型的架构？  

为此本文设计了一种 Transformer 型的模块，称为上下文 Transformer (Contextual Transformer, CoT)。  

## 模型架构

![Fig 2](/images/2021/PRN89/2.png)

### Contextual Transformer Block

K 在 \\(k \times k\\) 个 grid 中进行 \\(k \times k\\) 组卷积得到静态上下文信息 \\(K^1\\)，然后与 \\(Q\\) 拼接，进而由 \\(1 \times 1\\) 卷积 \\(\rightarrow\\) ReLU \\(\rightarrow\\) \\(1 \times 1\\) 卷积处理，最终与 \\(V\\) 做哈达玛积得到最终的特征图 \\(K^2\\)，称 \\(K^2\\) 为输入特征的动态上下文表征。  

### Contextual Transformer Networks

可以用 CoT 替换卷积块。  

#### ResNet-50
![Tab 1](/images/2021/PRN89/T1.png)

#### ResNeXt-50
![Tab 2](/images/2021/PRN89/T2.png)
