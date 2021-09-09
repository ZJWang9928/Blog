---
title: "[论文阅读笔记 -- 跨模态哈希] Aggregation-based Graph Convolutional Hashing (TMM 2021)"
date: 2021-09-09T13:11:23+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "graph", "hashing", "unsupervised"]
draft: false
---

# Aggregation-based Graph Convolutional Hashing for Unsupervised Cross-modal Retrieval (TMM 2021)

## 概述

本文提出基于聚合的图卷积哈希方法 (Aggregation-based Graph Convolutional Hashing, AGCH)。  

![Fig 1](/images/2021/PRN92/1.png)

## 模型架构

### 主要部件
+ 图像编码器 + 图像 GCN
+ 文本编码器 + 文本 GCN
+ 融合模块

生成各模态的二进制编码：  
$$B^v = sign(f(o^v; \theta_{v})),$$
$$B^t = sign(f(o^t; \theta_{t})).$$

在训练阶段符号函数 \\(sign(\cdot)\\) 会导致梯度问题，因而用 \\(tanh(\cdot)\\) 代替。  

拼接后融合：  

$$o^h = f^v \oplus f^t,$$
$$B^h = tanh(f(o^h; \theta_{h})).$$

同时也用 GCN 进行处理：  
$$B^gv = sign(f(f^v; \theta_{gv})),$$
$$B^gt = sign(f(f^t; \theta_{gt})).$$

## 相似度矩阵的构建

结合余弦相似度和欧几里德距离：  

$$S_{ij} = C_{ij} \cdot D_{ij} = ((\widetilde{Z}\_{i\*})^T \widetilde{Z}\_{j\*}) \cdot exp(-\sqrt{||\widetilde{Z}\_{i\*} - \widetilde{Z}\_{j\*}||\_{2}}/\rho).$$

## 具体算法

![Alg 1](/images/2021/PRN92/A1.png)
