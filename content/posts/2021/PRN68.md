---
title: "[论文阅读笔记 -- 跨模态检索 / 图网络] Adapted Graph Reasoning and Filtration (SIGIR 2021)"
date: 2021-08-02T16:52:49+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "graph", "GCN"]
draft: false
---

# Adapted Graph Reasoning and Filtration for Description-Image Retrieval (SIGIR 2021)

![Fig 1](/images/2021/PRN68/1.png)

## 概述

本文处理更加抽象的文本描述。  

提出一种自适应的图推理与过滤网络 (Adapted Graph Reasoning and Filtration network, AGRF)，包含两大主要部件：  

+ 自适应图推理网络 (Adapted Graph Reasoning network, AGR)，构建为图像上的目标关系图，在边和节点上均逐步推理，整体的图像表征由推理得到
+ 跨模态细粒度门控过滤 (Cross-Modal Fine-grained Gated Filtration, GF)

![Fig 2](/images/2021/PRN68/2.png)

## 特征提取

### 视觉表征

Faster R-CNN  

### 文本表征

BERT  

## 自适应图推理与聚合

以图像区域为节点构建图，在每一步动态更新节点之间的关联：  

$$e_{i}^0 = v_{i},$$
$$z_{i}^t = W_{a}^t e_{i}^{t-1} + b^t,$$
$$\theta_{i, j}^t = LeakyReLU(\phi^T[z_{i}^t || z_{j}^j]),$$
$$a_{i, j}^t = softmax(\theta_{i, j}^t),$$
$$e_{i}^t = LeakyReLU(\sum_{i=k}^N a_{k,i}^t z_{k}^t) + e_{i}^{t-1}.$$  

## 跨模态细粒度门控过滤

$$g_{i} = \sigma(W_{g}(e_{i}^T + T)),$$
$$e_{i}^{\*} = g_{i} \odot e_{i}^T.$$  

均值池化得到最终的整体图像表征。  

## 损失函数

三元组损失。  
