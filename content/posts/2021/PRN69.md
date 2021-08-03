---
title: "[论文阅读笔记 -- 跨模态哈希 / 图网络] Determining the Semantic Graph Connectivity (IJCAI 2020)"
date: 2021-08-03T11:07:47+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "graph", "GCN"]
draft: false
---

# Set and Rebase: Determining the Semantic Graph Connectivity for Unsupervised Cross-Modal Hashing (IJCAI 2020)

![Fig 1](/images/2021/PRN69/1.png)

## 概述

### 两类无监督跨模态哈系
+ 跨模态量化，最小化二进制编码与原始数据低维投影之间的鸿沟
+ 跨模态相似度搜索

### 三个主要问题
+ 在没有标签的情况下衡量数据语义关系的困难
+ 数据相似度在不同模态不一致的问题
+ 训练过程中数据语义关联的稀疏性问题

本文提出一种新的无监督方法，称为 Semantic-Rebased Cross-modal Hashing (SRCH)，采用 Set-and-Rebase 的方式学习语义图。  

![Fig 2](/images/2021/PRN69/2.png)

## Sparse Graph Setting and Rebasing

### Set the Semantic Geometric Prior

在不同模态构建几何稀疏图结构，这些几何图在训练时固定，采用 KNN 构建。  

### Rebase the Graph by Code Similarity

用处于 0 到 1 之间的连续实数表征一对样本之间的语义关联概率，而非在使用二值。  

概率值根据两个样本之间的相似度学到，计算如下  

$$S_{(i, j)} = p(e_{i, j} = 1) = \Phi(x_{i}, x_{j}).$$  

## 目标函数

### 自编码损失

$$\mathcal{L}\_{g}^A = ||W_{g}X_{g} - B||\_{F}^2 + ||X_{g} - W_{g}^TB||\_{F}^2,$$
$$s.t. \ B \in \\{+1, -1\\}^{l \times n}, BB^T = nI, W{g}^TW_{g} = I, g \in \\{V, T\\}.$$  

### 语义损失

$$\mathcal{L}\_{g}^S = \sum\_{(i, j) \in \epsilon\_{g}} C_{g(i, j)} ||S_{(i, j)}(z_{i} - z_{j})||\_{2}^2,$$
$$C_{g(i, j)} = \frac{\frac{1}{N} \sum\_{m=1}^N a_{g(m)}}{\sqrt{a_{g(i)} a_{g(j)}}},$$
$$\mathcal{L}\_{Z} = \frac{1}{2} ||Z - B||\_{F}^2.$$

\\(a_{g(m)}\\) 表示图中第 \\(m\\) 个数据点的度。  

### 语义图稀疏性损失

$$\mathcal{L}\_{g}^R = \sum\_{(i,j) \in \epsilon\_{g}} C_{g(i, j)} (S_{(i, j)} - 1)^2.$$  

## 优化过程

![Alg 1](/images/2021/PRN69/A1.png)

## 样本外编码计算

$$B_{g} = sign(W_{g}^{\*} X_{g}).$$
