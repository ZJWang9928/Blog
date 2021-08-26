---
title: "[论文阅读笔记 -- 跨模态检索] HAL: Mitigating Visual Semantic Hubs (AAAI 2020)"
date: 2021-08-26T12:55:43+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "hubness"]
draft: false
---

# HAL: Improved Text-Image Matching by Mitigating Visual Semantic Hubs (AAAI 2020)

![Fig 1](/images/2021/PRN83/1.png)

## 背景

本文主要针对图文匹配任务中的[枢纽点问题 (Hubness Problem)](http://jonathanwayy.xyz/2021/ldp_hubness_problem/) 进行研究。  

由于图文匹配中的嵌入空间是由联合建模视觉和语言得到的，通常将其成为视觉语义嵌入 (Visual Semantic Embeddings, VSE)。  

本文提出一种自适应的枢纽点感知损失 (self-adjustable hubness-aware loss)，称为 HAL，其既考虑从整个训练集采样而来的全局统计量，也考察从 mini-batch 得到的局部统计量，利用枢纽的信息自动调整采样权重。其既从困难样本进行学习，也由于考虑了多个样本而对噪声健壮。  

为了决定权重，对一个样本考察如下两种关系：  
+ 其与 mini-batch 内样本的关系
+ 其与一个 memory bank 中 k 近邻 query 的关系

## 两种基于三元组的损失函数

### Sum-margin Loss (SUM)

最大的不足在于平等看待 mini-batch 中所有的三元组，为之赋予相同的权重。  

### Max-margin Loss (MAX)

其梯度容易收到噪声的主导，训练数据中的伪最困难负样本 (pseudo hardest negatives) 是噪声的一个主要来源。  

二者均同时之考察一个匹配对和一个适配对。  

## The Hubness-Aware Loss (HAL)

### Neighborhood Component Analysis (NCA)

在分类任务中 NCA 定义为  

$$\mathcal{L}\_{NCA} = \sum_{i=1}^{N} (log \sum_{y_{i} = y_{j}} e^{S_{ij}} - log \sum_{k=1}^{N} e^{S_{ik}}),$$

其中 \\(N\\) 是样本数，\\(S\\) 表示余弦相似度。  

对于一个样本 \\(S_{ij}\\)，当它与搜索空间中多个样本是近邻，即是一个 hub 时，其作为正样本的权重降低，作为负样本的权重升高，意味着该样本在训练中会受到更多注意。  

### Global Weighting through Memory Bank (MB)

在每个 epoch 的最初，计算整个训练集样本的嵌入，构建 memory bank \\(M\\)，接着利用 mini-batch 和 memory bank 之间的关系为 batch 中各样本计算一个全局权重，用于强调 hub 并将权重传到接下来的 local weighting 阶段。  

定义一个函数 \\(kNN(x, M, k)\\) 返回点集 \\(M\\) 中 \\(x\\) 基于 \\(l_{2}\\) 距离的 \\(k\\) 近邻，HAL 的 global weighting 定义如下：  

![Eq 1](/images/2021/PRN83/Eq1.png)

其中 \\(K_{1} = kNN(i, M_{T} \backslash \\{t\\}, k), K_{2} = kNN(t, M_{I} \backslash \\{i\\}, k)\\)。  

### Local Weighting through Loss Function

$$\mathcal{L}\_{HAL} = \frac{1}{N} \sum_{i=1}^{N} (\frac{1}{\gamma} log(1 + \sum_{m \ne i} e^{\gamma W_{mi}(S_{mi} - \epsilon)}) + \frac{1}{\gamma} log(1 + \sum_{n \ne i} e^{\gamma W_{in}(S_{in} - \epsilon)}) - log(1 + W_{ii} S_{ii})),$$

其中 \\(\epsilon\\) 是 margin 值。  
