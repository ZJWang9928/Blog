---
title: "[论文阅读笔记 -- 跨模态检索] Learning Joint Embedding of Food Images and Recipes (TMM 2021)"
date: 2021-08-08T20:31:27+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention"]
draft: false
---

# Cross-Modal Food Retrieval: Learning a Joint Embedding of Food Images and Recipes with Semantic Consistency and Attention Mechanism (TMM 2021)

![Fig 1](/images/2021/PRN73/1.png)

## 核心

相同食物的数据表征可能不同，而不同食物的数据表征可能相似，从而导致食物数据有较大的类内方差与较小的类间方差。  

现有的研究只是利用三元组损失来解决类间方差小的问题，但是由于类内的平均距离仍然较大，会导致聚类松散。  

不同食物的菜谱中有公共成分。  

本文提出一种语义一致的基于注意力的网络 (Semantic-Consistent and Attention-Based Network, SCAN)。  

![Fig 2](/images/2021/PRN73/2.png)

## 特征编码

### 文本编码

LSTM + 自注意力

![Fig 3](/images/2021/PRN73/3.png)

### 图像编码

ResNet-50  

## 跨模态食品检索学习

基于三元组损失，用欧几里德距离。  

## 语义一致性 (Semantic Consistency)

可以理解为在一般的 ID loss 基础上，在两个似然之间计算双向的 KL 散度。  

## SCAN 伪代码

![Alg 1](/images/2021/PRN73/A1.png)
