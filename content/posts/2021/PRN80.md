---
title: "[论文阅读笔记 -- 图网络 / Ad-hoc 检索] A Graph-based Relevance Matching Model (AAAI 2021)"
date: 2021-08-17T15:53:21+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "graph", "GCN"]
draft: false
---

# 2101.11873 A Graph-based Relevance Matching Model for Ad-hoc Retrieval (AAAI 2021)

![Fig 1](/images/2021/PRN80/1.png)

## 背景

query-document 检索任务。  

### 两类架构
+ representation-based matching
+ interaction-based matching

### 两种重要关系
+ term-level query-document interaction
+ document-level word relationships

### 一个问题
用来检索的短语可能并不连续出现在文档中，可能相隔甚远。  

本文用图结构来处理该问题，设计了一种基于图的关联性匹配模型 (Graph-based Relevance Matching Model, GRMM)。  

![Fig 2](/images/2021/PRN80/2.png)

## 图的构建

### 节点特征

文档中单词特征作为节点，计算余弦相似度矩阵作为交互矩阵 \\(S \in \mathbb{R}^{n \times M}\\)：  

$$S_{ij} = cosine(e_{i}^{(d)}, e_{j}^{(q)}).$$  

### 拓扑结构

邻接矩阵 \\(A \in \mathbb{R}^{n \times n}\\) 定义为：  

$$A_{ij} = \begin{cases} count(i, j) \quad if \ i \ne j \\\\ 0 \qquad \qquad \quad otherwise \end{cases},$$

其中 \\(count(i, j)\\) 为两个单词在同一滑窗中共同出现的次数。  

进而对 \\(A\\) 作归一化，较常规。  

## 基于图的匹配

### 邻居聚合

$$h_{i}^0 = S_{i,:}, \ i = 1, 2, \cdots, n,$$
$$a_{i}^{t} = \sum_{(w_{i}, w_{j}) \in \epsilon} \widetilde{A}\_{ij}W_{a}h_{j}^t.$$

### 状态更新

类似 GRU 的形式根据 \\(a\\) 更新 \\(h\\)。  

### 特征选取

采用 k-max-pooling 策略选取特征。  

$$H = h_{1}^t || \cdots || h_{n}^t,$$
$$x_{j} = topk(H:,j).$$  
