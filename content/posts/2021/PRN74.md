---
title: "[论文阅读笔记 -- 注意力机制] CCNet: Criss-Cross Attention (ICCV 2019)"
date: 2021-08-09T12:11:40+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention"]
draft: false
---

# CCNet: Criss-Cross Attention for Semantic Segmentation (ICCV 2019)

[开源代码传送门](https://github.com/speedinghzl/CCNet)

![Fig 1](/images/2021/PRN74/1.png)

## 概述

提出十字注意力 (Criss-Cross Attention)，将参数量从非局部注意力的 \\(H \times W\\) 减少到了 \\(H + W -1\\)。  

为了捕捉全图依赖关系，对十字注意力模块采用递归操作，共享各模块的参数以减少参数量。  

将十字注意力模块嵌入到 CNN 中，称为 CCNet。  

![Fig 2](/images/2021/PRN74/2.png)

## 十字注意力

### Affinity 操作

用于生成注意力图 \\(A \in \mathbb{R}^{(H + W - 1) \times W \times H}\\)，位置 \\(u\\) 上的 \\(Q\\) 为 \\(Q_{u} \in \mathbb{R}^{C'}\\)，通过从 \\(K\\) 提取与位置 \\(u\\) 同一行及同一列的特征向量得到集合 \\(\Omega_{u} \in \mathbb{R}^{(H + W - 1) \times C'}\\)。Affinity 操作定义为  

$$d_{i, u} = Q_{u}\Omega_{i, u}^T,$$  

其中 \\(d_{i, u} \in D\\) 是两个特征之间的相关程度，\\(D \in \mathbb{R}^{(H + W -1) \times W \times H}\\)。进而沿着 \\(D\\) 的通道维度做 softmax 得到注意力图 \\(A\\)。  

### Aggregation 操作

\\(V\\) 中与位置 \\(u\\) 同一行及同一列的特征向量集合为 \\(\Phi_{u} \in \mathbb{R}^{(H + W - 1) \times C}\\)，通过 Aggregation 操作收集上下文信息：  

$$H_{u}' = \sum_{i \in |\Phi_{u}|} A_{i, u}\Phi_{i, u} + H_{u}.$$  


![Fig 3](/images/2021/PRN74/3.png)

## 信息传递示例

![Fig 4](/images/2021/PRN74/4.png)

## 结果示例

![Fig 5](/images/2021/PRN74/5.png)

## 注意力可视化

![Fig 6](/images/2021/PRN74/6.png)
