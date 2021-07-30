---
title: "PRN64"
title: "[论文阅读笔记 -- 跨模态检索] Retrieve Fast Rerank Smart: Cooperative and Joint Approaches (2021)"
date: 2021-07-30T11:12:23+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "cross-modal", "retrieval"]
draft: false
---

# 2103.11920v1 Retrieve Fast, Rerank Smart: Cooperative and Joint Approaches for Improved Cross-Modal Retrieval (2021)

[开源代码传送门](https://github.com/UKPLab/MMT-Retrieval)

![Fig 1](/images/2021/PRN64/1.png)

## 核心想法

要在计算效率和模型精度上找到平衡。  

## 四种模型

### Cross-Encoders

如图 1(a) 所示。  

二分类问题，将 \\([CLS]\\) token 输入分类器，用交叉熵训练。  

表现会比较好，但是计算成分高。  

### Embedding-Based Retrieval Methods

如图 1(b) 所示。  

分别提取特征，池化得到表征，三元组损失训练。  

计算成本低，但是表现可能相对差。  

### Separate Training, Cooperative Retrieval

如图 1(c) 所示。  

用合作检索 (cooperative retrieval) 方法结合上述两种模型的优势。  

分别独立训练 CE 和 EMB 模型，检索过程分为两步。  

首先用 EMB 检索 top-k 相关目标，再用 CE 进一步重排。  

由于要同时将两个模型放入内存，该方法参数效率更低。  

### Joint Training, Cooperative Retrieval

如图 1(d) 所示。  

训练一个联合模型，既用于 cross-encode 也用于 embed，通过切换输入类型与损失来训练共享的参数。  

检索过程与上一节方法相同。  

## 结果示例

![Fig 2](/images/2021/PRN64/2.png)
