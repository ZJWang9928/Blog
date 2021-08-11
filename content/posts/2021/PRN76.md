---
title: "[论文阅读笔记 -- 图网络] Graph Reasoning Networks on a Similarity Pyramid (ICCV 2019)"
date: 2021-08-11T15:46:56+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "graph", "GCN"]
draft: false
---

# Fashion Retrieval via Graph Reasoning Networks on a Similarity Pyramid (ICCV 2019)

![Fig 1](/images/2021/PRN76/1.png)

## 概述

Fashion Retrieval 任务。  

一个问题在于对应位置的局部块通常是失配的，因此需要在同尺度所有局部之间遍历匹配。  

设计了一种基于相似度金字塔的图推理网络 (Graph Reasoning Network (GRNet) on a Similarity Pyramid)。  

![Fig 2](/images/2021/PRN76/2.png)

## 图推理网络

提取多尺度的特征。  

### 相似度金字塔图

计算相似度向量，而非标量相似度：  

$$S_{p}(x_{l}^i, y_{l}^j) = \frac{P|x_{l}^i - y_{l}^j|^2}{||P|x_{l}^i - y_{l}^j|^2||\_{2}}.$$

以此为节点，构建边  

$$w_{p}^{l_{1}ij, l_{2}mn} = \frac{exp((T_{out} s_{l_{1}}^{ij})^T (T_{in} s_{l_{2}}^{mn}))}{\sum_{l, p, q} exp((T_{out} s_{l_{1}}^{ij})^T (T_{in} s_{l}^{pq}))}.$$

### 相似度推理

$$\hat{s}\_{l_{1}}^{ij} = \sum_{l_{2}, m, n} w_{p}^{l_{1}ij, l_{2}mn} s_{l_{2}}^{mn},$$
$$h_{l_{1}}^{ij} = ReLU(W\hat{s}\_{l_{1}}^{ij}).$$  

## 特征可视化

![Fig 3](/images/2021/PRN76/3.png)
