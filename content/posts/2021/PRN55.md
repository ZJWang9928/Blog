---
title: "[论文阅读笔记 -- ViT] Incorporating Convolution Designs into Visual Transformers (2021)"
date: 2021-07-20T10:06:20+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT"]
draft: false
---

# 2103.11816 Incorporating Convolution Designs into Visual Transformers (2021)

![Fig 1](/images/2021/PRN55/1.png)

## 核心

设计一种通过卷积增强的图像 Transformer (Convolution-enhanced image Transformer, CeiT)，将 CNN 提取低级特征、强化局部性与 Transformer 提取长程依赖的优势相结合。  


## Image-to-Tokens 模块

$$x' = I2T(x) = MaxPool(BN(Conv(x))).$$

![Fig 2](/images/2021/PRN55/2.png)

## 局部增强的前馈网络

提出一种局部增强的前馈网络 (Locally-enhanced Feed-Forward Network, LeFF) 层，结合 CNN 与 Transformer 的优势。  

![Fig 3](/images/2021/PRN55/3.png)

## Layer-wise Class-Token Attention (LCA)

取各层的 class token 拼接作为输入。  

![Fig 4](/images/2021/PRN55/4.png)
