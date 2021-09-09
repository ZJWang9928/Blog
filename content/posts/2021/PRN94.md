---
title: "[论文阅读笔记 -- 3D 跨模态检索] Cross-Modal Center Loss for 3D CM Retrieval (CVPR 2021)"
date: 2021-09-09T22:52:53+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "3D"]
draft: false
---

# Cross-Modal Center Loss for 3D Cross-Modal Retrieval (CVPR 2021)

![Fig 1](/images/2021/PRN94/1.png)

## 概述

### 现有方法的问题

+ 核心想法是最小化由预训练模型提取的特征之间的跨模态差异，预训练模型没有参与训练或进行微调
+ 现有的损失函数主要是为两种模态的情况设计的，无法很好处理超过两种模态数据的情况

本文提出一种跨模态中心损失 (Cross-modal Center Loss)，针对多模态间类间差异的最小化而设计，在公共空间中为各个类学习一个唯一的中心 \\(C\\)。  

![Fig 2](/images/2021/PRN94/2.png)

## 损失函数

### 跨模态中心损失

对于 \\(N\\) 个实例 \\(M\\) 个模态，计算如下  

$$L_{c} = \frac{1}{2} \sum_{i=1}^N sum_{m=1}^M ||v_{i}^m - C_{y_{i}}||\_{2}^2.$$

大的 batch size 能够学习一个更加健壮的中心。  

中心在训练时首先定义为一个 mini-batch 中某一类别对应特征的均值，进而由优化器进行更新。  

### 鉴别损失

标签空间中的交叉熵损失：  

$$L_{d} = -\frac{1}{N} (\sum_{i=1}^N \sum_{m=1}^M y_{i}^m \cdot log(\hat{y}\_{i}^m)).$$

### 减小各实例的跨模态差异

均方误差：  

$$L_{m} = \sum_{\alpha, \beta \in [1, M], \alpha \ne \beta} ||v_{i}^{\alpha} - v_{i}^{\beta}||\_{2}^{2}.$$

## 优化过程

![Alg 1](/images/2021/PRN94/A1.png)
