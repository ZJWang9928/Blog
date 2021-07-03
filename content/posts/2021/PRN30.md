---
title: "[论文阅读笔记 -- 注意力机制] VOLO: Vision Outlooker for Visual Recognition (2021)"
date: 2021-07-03T16:23:02+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "adversarial", "GAN", "adversarial samples"]
draft: false
---

# 2106.13112v2 VOLO: Vision Outlooker for Visual Recognition (2021)

![Fig 1](/images/2021/PRN30/1.png)

## 背景

### 制约 ViT 不如 CNN 的一个主要因素

ViT 在将细粒度特征以及上下文编码成 token 时效率较低。  

本文设计了一种简单轻量的注意力机制，称为 Outlooker，通过高效的线性投影由一个锚点 token 特征聚合其周遭 token，避免了点乘注意力昂贵的计算量。  

![Fig 2](/images/2021/PRN30/2.png)

## Outlooker

Outlooker 由两部分组成：  

+ outlook 注意力层：编码空间信息
+ 多层感知机 (MLP)：进行通道间信息交互

对于 \\(X \in \mathbb{R}^{H \times W \times C}\\)，有：  

$$\widetilde{X} = OutlookerAtt(LN(X)) + X,$$
$$Z = MLP(LN(\widetilde{X})) + \widetilde{X},$$

其中 \\(LN\\) 表示 LayerNorm。  

### Outlook 注意力

#### 其背后的主要观点
+ 处于各空间位置的特征具有足够的表征能力为局部聚合其邻域特征生成注意力权重
+ 密集的局部空间聚合 (dense and local spatial aggregation) 能够有效编码细粒度信息

对于各空间位置 \\((i, j)\\) outlook 注意力在一个以该位置为中心的 \\(K \times K\\) 窗口中计算其与所有邻居的相似度。不同于自注意力需要的 Query-Key 矩阵乘法，outlook 注意力只需要简单的变形操作 (reshaping operation) 即可实现。  

对于输入的 \\(X \in \mathbb{R}^{H \times W \times C}\\)，首先使用两个线性层，将各 \\(C\\) 维的 token 投影为 outlook 权重 \\(A \in \mathbb{R}^{H \times W \times K^{4}}\\) 以及值特征 (value representation) \\(V \in \mathbb{R}^{H \times W \times C}\\)。用 \\(V_{\Delta_{i,j}} \in \mathbb{R}^{C \times K^{4}}\\) 表示以 \\((i, j)\\) 为中心的局部窗口中的所有值。  

将 \\(A\\) 变形到 \\(\hat{A} \in \mathbb{R}^{K^2 \times K^2}\\)，值投影过程为：  

$$Y_{\Delta_{i, j}} = MatMul(Softmax(\hat{A}_{i, j}), V\_{\Delta\_{i, j}}).$$  

进而对特征密集聚合 (dense aggregation)：  

$$\widetilde{Y}\_{i, j} = \sum\_{0 \le m, n < K} Y\_{\Delta\_{i+m- \lfloor \frac{K}{2} \rfloor, j+n - \lfloor \frac{K}{2} \rfloor}}^{i, j}.$$

PyTorch 形式的代码实现如下：  

![Code 1](/images/2021/PRN30/C1.png)

### 多头 Outlook 注意力

只需要将线性层维度对应多头数翻倍即可实现。  

分别计算，最后拼接。  

## 一些注意力机制的对比

|                                | Query | Key   | Value |
|--------------------------------|-------|-------|-------|
| Transformer                    | seq   | seq   | seq   |
| MLP-Mixer (External Attention) | seq   | param | param |
| Outlook Attention              | seq   | param | seq   |
