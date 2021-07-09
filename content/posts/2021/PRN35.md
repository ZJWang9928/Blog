---
title: "[论文阅读笔记 -- ViT] Swin Transformer (2021)"
date: 2021-07-09T10:54:19+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT"]
draft: false
---

# 2103.14030 Swin Transformer: Hierarchical Vision Transformer using Shifted Windows (2021)

[开源代码传送门](https://github.com/microsoft/Swin-Transformer)

![Fig 1](/images/2021/PRN35/1.png)

## 背景

### Transformer 在视觉任务上的主要困难
+ 视觉元素的尺度可能相当不同，但是当前工作中 token 都固定尺度
+ 图像具有更高的像素分辨率  

本文提出一种新的架构，称为 Swin Transformer，其构建层叠化的特征图，并且关于图像尺寸为线性计算复杂度。  

![Fig 2](/images/2021/PRN35/2.png)

### 滑动窗口

Swin Transformer 的核心部件在于其滑窗 (shifted window) 方法，将前一层窗口联系起来，提高其建模能力。  

![Fig 3](/images/2021/PRN35/3.png)

## 基于自注意力的滑动窗口

### 不重叠窗口中的自注意力

在各局部窗口中计算自注意力，各窗口包含 \\(M \times M\\) 个 patch，其计算复杂度关于 patch 总数 \\(hw\\) 是线性的，而全局的自注意力复杂度则是二阶的。  

### 连续块间的滑窗划分

在前一层的邻接非重叠窗口之间形成连接。  

$$\hat{z}^{l} = WMSA(LN(Z^{l-1})) + z^{l-1},$$
$$z^{l} = MLP(LN(\hat{z}^{l})) + \hat{z}^{l},$$
$$\hat{z}^{l+1} = SWMSA(LN(Z^{l})) + z^{l},$$
$$z^{l+1} = MLP(LN(\hat{z}^{l+1})) + \hat{z}^{l+1},$$

### 高效的批量计算方法

![Fig 4](/images/2021/PRN35/4.png)

### 相对位置偏置 (Relative Position Bias)

在计算自注意力过程中，在各个 head 引入一个相对位置偏置 \\(B \in \mathbb{R}^{M^2 \times M^2}\\)：  

$$Attention(Q, K, V) = softMax(QK^T / \sqrt{d} + B)V,$$  

其中 \\(Q, K, V \in \mathbb{R}^{M^2 \times d}\\)，\\(M^2\\) 为一个窗口中的 patch 数。  
