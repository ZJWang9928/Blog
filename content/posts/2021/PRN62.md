---
title: "[论文阅读笔记 -- 联邦学习 / 跨模态检索] FedCMR: Federated Cross-Modal Retrieval (SIGIR 2021)"
date: 2021-07-28T14:36:28+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "federated learning", "cross-modal", "retrieval"]
draft: false
---

# FedCMR: Federated Cross-Modal Retrieval (SIGIR 2021)

![Fig 1](/images/2021/PRN62/1.png)

## 概述

基于深度学习的方法需要大量高质量的多模态数据，而现实中，多模态数据由许多不同用户 (client) 分别生成。  

本文研究联邦跨模态检索 (Federated Cross-Modal Retrieval, FedCMR)。  

### FedCMR 三大步骤

+ 局部训练：各用户用局部数据训练跨模态检索模型
+ 聚合：服务器聚合用户的公共空间
+ 局部更新：各用户基于聚合后的模型更新局部模型的公共子空间

![Fig 1](/images/2021/PRN62/2.png)

## 局部跨模态检索网络

用 DSCMR 作为局部模型处理各用户的多模态数据。  

## 模型聚合

### 聚合架构 (Aggregation Framewor)

由于跨模态网络使用公共子空间来连结多个模态，本文通过利用公共空间，在用户之间共享知识。  

服务器收集学习公共空间的线性层参数，并如下组合所有公共子空间的更新，从而找到一个全局一致的公共空间：  

$$W_{t} = \sum\_{k=1}^K q_{t}^kW_{t}^k,$$  

其中 \\(K\\) 是第 \\(t\\) 次信息通信时选中的用户数量

### 权重策略 (Weighting Scheme)

权重与用户拥有的样本数以及标签类别数成正比，即

$$q_{t}^k \propto S^k,$$
$$S^k = \frac{n^k}{\sum\_{k=1}^K n^k} \cdot \frac{c^k}{\sum\_{k=1}^K c^k}.$$  

又因为每轮通信时，在相同的损失计算机制下，损失值小的用户检索能力更强，因而权重因与损失值成反比，收到 Gompertz 函数的启发为各用户计算 \\(V_{t}^k\\) 如下：  

$$V_{t}^k = e^{-e^{f_{t}^K/(\frac{\sum\_{k=1}^K f_{t}^k}{K})}},$$

其中 \\(f_{t}^k\\) 表示损失值，有  

$$q_{t}^k \propto V_{t}^k.$$  

最终，权重计算如下：  

$$q_{t}^k = \frac{e^{S^k} + \alpha V_{t}^k}{\sum\_{k=1}^K e^{S^k} + \alpha V_{t}^k}.$$  

## 局部更新

### 步骤
+ 各轮通信的最初用户记录局部公共空间 \\(W_{0}^k\\)
+ 各用户用局部数据训练模型得到局部公共子空间 \\(W_{t}^k\\) 并上传到服务器，服务器返回聚合后的公共子空间 \\(W_{t}\\)
+ 用户收到 \\(W_{t}\\) 后如下更新局部子空间：

$$W_{t + 1}^k = W_{t} + \gamma(W_{t}^k - W_{0}^k).$$  

## 算法

训练过程分为两个阶段，联合训练 (joint training) 与 独立增强训练 (independent enhancement training)。  

![Alg 1](/images/2021/PRN62/A1.png)
