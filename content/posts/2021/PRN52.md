---
title: "[论文阅读笔记 -- 跨模态检索 / 图网络] Graph Structured Network (CVPR 2020)"
date: 2021-07-18T10:26:07+08:00
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

### 节点级别匹配

### 结构级别匹配
