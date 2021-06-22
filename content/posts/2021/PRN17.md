---
title: "[论文阅读笔记 -- 跨模态检索] Cross-Modal Center Loss (CVPR 2021)"
date: 2021-06-22T19:53:32+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid", "retrieval"]
draft: false
---

# Cross-Modal Center Loss for 3D Cross-Modal Retrieval (CVPR 2021)

![Fig 1](/images/2021/PRN17/1.png)

## 现有方法的问题
+ 核心思想是最小化由预训练网络提取的多模态特征之间的跨模态差异，而这些预训练网络应当与跨模态数据联合训练
+ 现有的损失函数主要针对两种模态的情况设计，当有更多模态时无法很好泛化

故此本文提出一种新的损失函数，称为跨模态中心损失 (Cross-modal Center Loss)，用于最小化跨越多种模态的类内差异。  

与传统的中心损失在每个模态空间孤立学习一个中心不同，跨模态中心损失在公共空间里为各个类学习一个中心 \\(C\\)。  

此外，本文通过元数据 (metadata) 联合训练整个网络架构，而非受到预训练模型的限制。  

本文使用的多模态数据包括网格 (Mesh)、点云 (point cloud)和图像，其特征分别由不同子网络提取，进而由共享的两层 FC 投影到公共空间。  

![Fig 2](/images/2021/PRN17/2.png)

## 跨模态中心损失 (Cross-modal Center Loss)

对于提取得到的特征 \\(\\{v_{i}^{m}\\}_{i=1}^N (m \in [1, M])\\)，共有 N 个实例和 M 个模态，跨模态中心损失定义如下：  

$$L _{c} = \frac{1}{2}\sum _{i=1}^{N} \sum _{m=1}^M ||v _{i}^m - C _{y _{i}}|| _{2}^2,$$  

其中 \\(C _{y _{i}} \in \mathbb{R}^k\\) 表示类别 \\(y _{i}\\) 在公共空间的中心， \\(k\\) 是特征维度。

每次训练迭代后，各个类中心 \\(C _{j}\\) 通过 \\(\Delta C _{j}\\) 由来自所有模态属于类别 \\(j\\) 的数据更新：  

$$\Delta C _{j} = \frac{\sum _{i=1}^N \sum _{m=1}^M \delta(y _{i} = j)(C _{j} - v _{i}^m)}{1 + \sum _{i=1}^N \delta(y _{i} = j)},$$

$$\delta(condition) = \begin{cases} 1 \ if \ condition=True, \\\\ 0, \ if \ otherwise, \end{cases}$$

Batch Size 够大时能够为每个类学习到一个健壮的中心。  

## 鉴别损失

为了学习具有鉴别能力的特征，标签空间中的交叉熵损失被用于优化网络，有点类似与 ID Loss。  

$$L _{d} = -\frac{1}{N}(\sum _{i=1}^N \sum _{m=1}^M y _{i}^m \cdot log(\hat{y _{i}^m})),$$

$$\hat{y _{i}^m} = MLP(v _{i}^m).$$

为了进一步减小各实例数据中的跨模态差异，采用一种基于均方误差 (mean square error) 的损失函数，最小化所有跨模态样本对特征之间的距离：  

$$L _{m} = \sum _{\alpha, \beta \in [1, M], \alpha \ne \beta}||v _{i}^{\alpha} - v _{i}^{\beta}|| _{2}^2.$$

## 优化过程

![Alg 1](/images/2021/PRN17/A1.png)

## Batch Size 的影响

每次各类的中心定义为一个 mini-batch 中相应特征的均值，用于更新，实验结果表明 Batch Size 的大小对模型表现有显著影响。  

![Tab 1](/images/2021/PRN17/T1.png)
