---
title: "[论文阅读笔记 -- 跨模态哈希] SDMCH: Supervised Discrete Manifold-Embeded (IJCAI 2018)"
date: 2021-09-16T15:23:30+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "retrieval", "hashing", "manifold"]
draft: false
---

# SDMCH: Supervised Discrete Manifold-Embedded Cross-Modal Hashing (IJCAI 2018)

## 概述

本文提出离散流形嵌入跨模态哈希方法 (Discrete Manifold-Embedded Cross-Modal Hashing, SDMCH)。  

既挖掘数据的非线性流形结构，也在多模态之间构建相关性。  

## 流形结构学习

首先借助局部线性嵌入 (Locally Linear Embedding) 构建相似度矩阵，一个点由其 K 近邻的线性加权组合得到：  

$$min_{P^{t}} \sum_{i=1}^{n} ||x_{i}^{t} - \sum_{j}^{K} P_{ij}^{t} x_{j}^{t}||\_{F}^{2}, \ s.t. \ \sum_{j} P_{ij}^{t} = 1,$$

\\(t\\) 是模态序号，若非邻居则 \\(P_{ij}^{t}\\) 取 0，由上式学到 \\(P^{t}\\)。

保留流形结构的相似度矩阵定义为 \\(S_{ij}^{t} = |P_{ip}^{t}|\\) 若 \\(x_{j}^{t}\\) 是 \\(x_{i}^{t}\\) 的第 \\(p\\) 个邻居，否则置零。  

进一步更新 \\(S^{t} \leftarrow ((S^{t})^T + S^{t})\\) 以确保对称性。  

## 目标函数

### 流形嵌入

$$min_{F, B} \sum_{t = 1}^{m} \lambda_{t} \sum_{i, j = 1}^{n} S_{ij}^{t} ||F_{i} - F_{j}||\_{F}^{2} + \alpha ||B - F||\_{F}^{2} = \sum_{t = 1}^{m} \lambda_{t} Tr(F^T L^t F) + \alpha ||B - F||\_{F}^{2},$$
$$s.t. \ B \in \\{-1, 1\\}^{n \times r}.$$

### 标签保持

$$min_{G} ||Y - BG||\_{F}^{2},$$

\\(G\\) 是将 \\(B\\) 映射到 \\(Y\\) 的投影矩阵。  

## 优化算法

![Alg 1](/images/2021/PRN98/A1.png)
