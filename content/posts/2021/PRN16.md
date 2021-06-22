---
title: "[论文阅读笔记 -- 跨模态哈希] Deep Cross-Modal Hashing (CVPR 2017)"
date: 2021-06-22T16:33:39+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid", "hashing"]
<!-- draft: false -->
draft: true
---

# Deep Cross-Modal Hashing (CVPR 2017)

## 哈希的目标
将原始空间数据点映射为汉明空间中的二进制编码，在汉明空间中保留原始空间中的相似度。  

## 两类多模态哈希 (Multi-Modal Hashing, MMH)

### 多源哈希 (multi-source hashing, MSH)

目的是使用来自所有模态的全部信息学习哈希编码，需要所有模态的全部数据点都能够被观测到，包括 query 点和数据库中的点。  

这在实际应用中十分受限，因为很难总是能够获取到所有模态的全部数据点。  

### 跨模态哈希 (cross-modal hashing, CMH)

Query 数据点和数据库中的点模态可以不同，因而应用更广泛。  

## 基于人工特征 CMH 方法的问题

特征提取过程独立于哈希编码学习过程，这意味着所提取的人工特征可能并非最适用于哈希编码学习阶段。  

因而本文提出一种端到端的深度跨模态哈希 (deep cross-modal hashing, DCHM) 方法，用于跨模态检索任务。  

与此前 CMH 方法将离散学习问题转化为连续学习问题不同， DCMH 直接学习离散哈希编码，以避免造成所学习到哈希编码精确度的损失。  

![Fig 1](/images/2021/PRN16/1.png)

## DCMH 模型

### 特征学习部分

用两个深度神经网络分别提取图文特征。  

![Tab 1](/images/2021/PRN16/T1.png)

![Tab 2](/images/2021/PRN16/T2.png)

### 哈希编码学习部分

实验发现，将来自两种模态的同一训练数据的二进制编码设为相同会有更好的表现。  

![Loss](/images/2021/PRN16/2.png)

#### 目标函数设计思路
+ 连续特征向量之间应当相似
+ 连续向量可以被视为二进制哈希编码的连续型替代 (continuous surrogate)，因此二者也应该接近 (Frobenius Norm)
+ 需保证各位上 +1 和 -1 数量的平衡，故最小化元素和的 F-Norm，从而最大化每一位能够提供的信息量

需要注意的是，仅仅在训练过程中的多模态哈希编码设为相同，实际应用时仍需要为 query 数据和数据库中数据分别生成一串编码。  

## 学习过程

每一轮都采用交替学习的策略 (alternating learning strategy) 学习图像分支参数、文本分支参数以及编码矩阵 \\(B\\)，每次只学习其中之一，将其余二者固定。  

+ 单独训练 V，固定 T 和 B
+ 单独训练 T，固定 V 和 B
+ 更新 B

具体算法描述如下：  

![Alg 1](/images/2021/PRN16/A1.png)

## 样本以外的延伸 (Out-of-Sample Extension)

对于任何不曾在训练集中出现过的数据点，只要其模态能被观测到，就可以得到其哈系编码：  

$$b = sign(f(x)).$$

## 基于哈希检索方法的评估标准

### Hamming Ranking

根据所给出的 query 数据点，计算数据库中数据点与其哈希编码之间的汉明距离，并由此排序。  

通常用 Mean Average Precision (MAP) 衡量这一标准下的模型精度。  

### Hash Lookup

找出所有落入 query 数据点 Hamming 半径范围内的数据点。  

通常使用 precision-recall curve 衡量模型表现。  
