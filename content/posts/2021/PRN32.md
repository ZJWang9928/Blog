---
title: "[论文阅读笔记 -- ViT] XCiT: Cross-Covariance Image Transformers (2021)"
date: 2021-07-05T12:59:51+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT"]
draft: false
---

# 2106.09681 XCiT: Cross-Covariance Image Transformers (2021)

[开源代码传送门](https://github.com/facebookresearch/xcit)

![Fig 2](/images/2021/PRN32/2.png)

## 背景

Transformers 中自注意力模块计算复杂度高。  

本文用一种转置的注意力 (transposed attention) 取代自注意力，称为交叉协方差注意力 (cross-covariance attention, XCA)，其对于 patch 数具有线性的复杂度，进而构建了交叉协方差图像 Transformers (Cross-Covariance Image Transformers, XCiT)。  

![Fig 1](/images/2021/PRN32/1.png)

## Gram 矩阵与协方差矩阵的关系

未归一化的 \\(d \times d\\) 协方差矩阵为 \\(C = X^TX\\)，\\(N \times N\\) 的 Gram 矩阵为 \\(G = XX^T\\)。  

二者特征谱 (sigenspectrum) 中的非零部分是等价的，其特征向量可以互相求得。若 \\(G\\) 的特征向量为 \\(V\\)，则 \\(C\\) 的特征向量为 \\(U=XV\\)。  

利用这一联系，本文试图避免计算 \\(N \times N\\) 注意力矩阵所带来的平方复杂度，该矩阵由 \\(G\\) 得到：  

$$QK^T = XW_{q}W_{k}^TX^T.$$  

本文考虑使用 \\(d_{k} \times d_{q}\\) 的 \\(C\\)：  

$$K^TQ = W_{k}^TX^TXW_{q}.$$  

## 交叉协方差注意力 (Cross-Covariance Attention)

沿着特征维操作，而非 token 维。  

$$XCAttention(Q, K, V) = V\mathcal{A}\_{XC}(K, Q),$$
$$\mathcal{A}\_{XC}(K, Q) = Softmax(\hat{K}^T\hat{Q} / \tau).$$  

对 query 和 key 矩阵的各列取 \\(\mathcal{l}\_{2}\\) 范数，能够提高训练的稳定性，但是由于限制了自由度会降低该操作的表征能力，因而引入一个可学习的 temperature 参数 \\(\tau\\)。  

### 对角线分块的交叉协方差注意力 (Block-diagonal Cross-Covariance Attention)

将特征分组，与多头思路类似。对各组分别使用该注意力，学习独立的权重矩阵。

#### 两个好处
+ 聚合 value 的复杂度进一步降低
+ 更容易优化，并带来结果上的提升  

与 Group Normalization 类似。  

#### 可视化呈现

![Fig 3](/images/2021/PRN32/3.png)

## Cross-covariance Image Transformers

### Local Patch Interaction (LPI) Block

在每个 XCA 块后加上 LPI 块，使得 patch 之间有显式的信息交互，由两个面向深度 (depth-wise) 的 \\(3 \times 3\\) 卷积层中间加 BN + GELU 构成。  

### 前向传播

Point-wise feed-forward network (FFN)，是单个包含 \\(4d\\) 个隐节点的隐藏层。  

XCA 块中特征在各组内交互优化，LPI 块因为面向深度，其中没有特征交互，FFN 使得所有特征之间能够进行交互。  

### 通过类注意力进行全局聚合

训练针对图像分类的模型时使用类注意力层 (class attention layers)。  
