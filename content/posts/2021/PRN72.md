---
title: "[论文阅读笔记 -- 注意力机制] Attentional Feature Fusion (WACV 2021)"
date: 2021-08-06T14:50:11+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention"]
draft: false
---

# 2009.14082 Attentional Feature Fusion (WACV 2021)

## 概述

本文研究特征融合 (feature fusion)，提出注意力特征融合模块 (attentional feature fusion module, AFF)。  

为了缓解由于尺度变化以及小目标引起的问题，提出注意力模块应还应当聚合来自针对不同尺度目标的不同感受野的上下文信息的观点，设计了多尺度通道注意力模块 (Multi-Scale Channel Attention Module, MS-CAM)。  

![Fig 1](/images/2021/PRN72/1.png)

## 多尺度通道注意力 (MS-CAM)

### SENet 中的通道注意力

$$w = \sigma(g(X)) = \sigma(BN(W_{2}ReLU(BN(W_{1}(GAP(X)))))).$$  

将各通道压缩成一个标量，过于粗糙，会忽略小目标。  

### 聚合局部与全局上下文

#### 核心观点

通道注意力可以通过变换空间池化的尺寸来多尺度地实现。  

用 point-wise 卷积 (PWConv) 作为局部通道上下文聚合器，为了减少参数量，局部通道上下文 \\(L(X) \in \mathbb{R}^{C \times H \times W}\\) 实现为 bottleneck 结构：  

$$L(X) = BN(PWConv_{2}(ReLU(BN(PWConv_{1}(X))))),$$ 

与输入特征形状相同。  

$$X' = X \otimes M(X) = X \otimes \sigma(L(X) \oplus g(X)).$$  

![Fig 2](/images/2021/PRN72/2.png)

## 注意力特征融合 (AFF)

$$Z = M(X \uplus Y) \otimes X + (1 - M(X \uplus Y)) \otimes Y,$$  

其中 \\(\uplus\\) 表示初始的特征聚合。  

## 特征融合方法总结

\\(G(\cdot)\\) 表示全局注意力机制。  

![Tab 1](/images/2021/PRN72/T1.png)

## 应用示例

![Fig 3](/images/2021/PRN72/3.png)
