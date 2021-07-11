---
title: "[论文阅读笔记 -- 注意力机制] SA-Net: Shuffle Attention for Deep CNNs (ICASSP 2021)"
date: 2021-07-11T10:53:48+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention"]
draft: false
---

# 2102.00240v1 SA-Net: Shuffle Attention for Deep Convolutional Neural Networks (ICASSP 2021)

[开源代码传送门](https://github.com/wofmanaf/SA-Net)

![Fig 1](/images/2021/PRN40/1.png)

## 背景

设计了 Shuffle Attention (SA) 模块，将特征沿着通道维分组，对每个子特征用 Shuffle 单元同时计算通道注意力与空间注意力。  

![Fig 2](/images/2021/PRN40/2.png)

## Shuffle Attention (SA)

### 特征分组

沿着通道维将特征图分为 \\(G\\) 组，各子特征输入到注意力单元时又首先被一分为二，分别用于生成通道注意力图与空间注意力图。  

### 通道注意力

$$s = \mathcal{F}\_{gap}(X_{k1}) = \frac{1}{H \times W}\sum_{i=1}^H \sum_{j=1}^W X_{k1}(i, j),$$
$$X_{k1}^{'} = \sigma(\mathcal{F}\_{c}(s)) \cdot X_{k1} = \sigma(W_{1}s + b_{1}) \cdot X_{k1}.$$  

### 空间注意力

$$X_{k2}^{'} = \sigma(W_{2} \cdot GN(X_{k2}) + b_{2}) \cdot X_{k2}.$$  

两个分支的结果拼接得到 \\(X_{k}^{'} = [X_{k1}^{'}, X_{k2}^{'}]\\)。  

### 聚合 (Aggregation)

最后聚合所有子特征，采用 channel shuffle 操作，沿着通道维形成跨组的信息交流。  

### SA 模块的 PyTorch 代码实现

![Alg 1](/images/2021/PRN40/A1.png)

## 效果与特征可视化

![Fig 3](/images/2021/PRN40/3.png)

![Fig 4](/images/2021/PRN40/4.png)
