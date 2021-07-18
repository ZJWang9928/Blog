---
title: "[论文阅读笔记 -- 跨模态检索 / 图网络] Graph Structured Network (CVPR 2020)"
date: 2021-07-18T10:10:07+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "cross-modal", "retrieval", "graph", "GCN"]
draft: false
---

# 2004.00277 Graph Structured Network for Image-Text Matching (CVPR 2020)

[开源代码传送门](https://github.com/CrossmodalGroup/GSMN)

![Fig 1](/images/2021/PRN52/1.png)

## 总览

本文提出一种图结构匹配网络 (Graph Structured Matching Network, GSMN)，对目标、关系与属性显式建模。  

### 以下两组相关性互相促进

+ 细粒度目标相关性 (fine-grained object correspondence)
+ 联系相关性 (relation correspondence) 与属性相关性 (attribute correspondence)

三者构成图，目标节点与联系节点或属性节点相连，在视觉图与文本图上均进行节点级别与结构级别的匹配。  

![Fig 2](/images/2021/PRN52/2.png)

## 图的构建

### 文本图 (Textual Graph)

文本图实现为无向稀疏图。  

首先用预训练模型 Standford CoreNLP 提取文本中的语义依赖关系，其除了能够解构句子中的目标（名词）、联系（动词）以及属性（形容词或量词）之外，还能解构这些元素之间的语义依赖。  

将各单词作为节点，若两单词之间有语义依赖则存在一条边。单词表征 \\(u\\) 的相似度矩阵 \\(S\\) 计算如下：  

$$s_{ij} = \frac{exp(\lambda u_{i}^Tu_{j})}{\sum_{j=0}^m exp(\lambda u_{i}^Tu_{j})}.$$  

边的权重矩阵如下计算得到：  

$$W_{e} = ||S \circ A||\_{2}.$$

此外，也将文本图实现为全连接图，比起利用单词语义依赖关系的稀疏图，其能够挖掘隐式依赖。  

能够发现，稀疏图与稠密图能互补以提升表现。  

### 视觉图 (Visual Graph)

将每张图像表征为无向全连接图，以 Faster-RCNN 检测到的显著区域作为节点。  

受到 VQA 上方法的启发，用极坐标建模各图的空间关系。  

## 多模态图匹配

目的在于匹配两张图以学习细粒度的相关性，生成相似度 \\(g(G_{1}, G_{2})\\) 作为一个图文对之间的相似度。  

文本图与视觉图的节点特征分别表示为 \\(U_{\alpha} \in \mathbb{R}^{m \times d}\\) 与 \\(V_{\beta} \in \mathbb{R}^{n \times d}\\)。  

### 节点级别匹配

#### 文本图上的节点级别匹配

根据相似度聚合视觉节点：  

$$C_{t \rightarrow i} = softmax_{\beta}(\lambda U_{\alpha}V_{\beta}^T)V_{\beta}.$$  

不同于此前方法使用学习到的相关性计算全局相似度，本文采用一种多块模块 (multi-block module) 计算文本节点与聚合后的视觉节点 \\(C_{t \rightarrow i}\\) 之间面向块的相似度 (block-wise similarity)，将相似度由标量变成了向量。  

将第 \\(i\\) 对节点均划分成 \\(t\\) 个块，分别计算余弦相似度后拼接：  

$$u_{i} = [u_{i1}, \cdots, u_{it}],$$
$$c_{i} = [c_{i1}, \cdots, c_{it}],$$
$$x_{ij} = cos(u_{ij}, c_{ij}),$$
$$x_{i} = x_{i1} || x_{i2} || \cdots || x_{it},$$  

\\(x_{i}\\) 表示第 \\(i\\) 个文本节点的匹配向量。

#### 视觉图上的节点级别匹配

视觉图的操作与文本图对称。  

聚合文本节点：  

$$C_{i \rightarrow t} = softmax_{\alpha}(\lambda V_{\beta}U_{\alpha}^T)U_{\alpha}.$$  

多块模块与上一节类似。  

### 结构级别匹配

利用 GCN 组合邻域匹配向量以更新各节点的匹配向量，得到联系相关性，GCN 层使用 \\(K\\) 个核：  

$$\hat{x}\_{i} = ||\_{k=1}^K \sigma (\sum_{j \in N_{i}} W_{e}W_{k}x_{j} + b),$$  

\\(W_{e}\\) 是边权重矩阵，卷积的结果拼接 \\(K\\) 个核的输出得到。  

用得到的相关性计算图文对的整体匹配分数，用 MLP 联合考虑全部相关性，表征一张结构化的图与另一张结构化的图有多匹配：  

$$s_{t \rightarrow i} = \frac{1}{n} \sum_{i} W_{s}^u (tanh(W_{h}^u \hat{x}\_{i} + b_{h}^u)) + b_{s}^u,$$
$$s_{i \rightarrow t} = \frac{1}{m} \sum_{j} W_{s}^v (tanh(W_{h}^u \hat{x}\_{j} + b_{h}^v)) + b_{s}^v,$$

图文对的整体匹配分数为：  

$$g(G_{1}, G_{2}) = s_{t \rightarrow i} + s_{i \rightarrow t}.$$  

## 目标函数

三元组损失：  

$$L = \sum_{(I, T)} [\gamma - g(I, T) + g(I, T')]\_{+} + [\gamma - g(I, T) + g(I', T)]\_{+}.$$  
