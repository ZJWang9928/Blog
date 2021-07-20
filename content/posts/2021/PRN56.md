---
title: "[论文阅读笔记 -- ViT] CMT: Convolutional Neural Networks Meet Vision Transformers (2021)"
date: 2021-07-20T14:32:30+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT"]
draft: false
---

# 2107.06263 CMT: Convolutional Neural Networks Meet Vision Transformers (2021)

![Fig 1](/images/2021/PRN56/1.png)

## 核心

本文设计了一种 CNN 与 transformer 的交集，即 CMT 架构。  

![Fig 2](/images/2021/PRN56/2.png)

## CMT 块

### Local Perception Unit (LPU)

绝对的位置编码破坏了平移不变性，忽视了 patch 中的局部关联与结构信息。  

$$LPU(X) = DWConv(X) + X.$$

DWConv 表示 depth-wise 卷积。  

### 轻量多头自注意力 (LMHSA)

用 \\(k \times k\\) depth-wise 卷积降低 \\(K\\) 和 \\(V\\) 的空间尺度，并加上一个随机初始化的可学习的相对位置偏置 \\(B\\)。  

$$LigntweightAttention(Q, K, V) = Softmax(\frac{QK'^T}{\sqrt{d_{k}}} + B)V'.$$  

### Inverted Residual Feed-forward Network

$$IRFFN(X) = Conv(\mathcal{F}(Conv(X))),$$
$$\mathcal{F}(X) = DWConv(X) + X.$$  

最终，CMT 块可以表示为：  

$$X'\_{i} = LPU(X_{i-1}),$$
$$X''\_{i} = LMHSA(LN(X'\_{i})) + X'\_{i},$$
$$X_{i} = IRFFN(LN(X''\_{i})) + X''\_{i}.$$  
