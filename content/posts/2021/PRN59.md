---
title: "[论文阅读笔记 -- 跨模态检索 / 图网络] Fine-grained Video-Text Retrieval with HGR (CVPR 2020)"
date: 2021-07-26T11:53:00+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "graph", "GCN"]
draft: false
---

# Fine-grained Video-Text Retrieval with Hierarchical Graph Reasoning (CVPR 2020)

[开源代码传送门](https://github.com/cshizhe/hgr_v2t)

![Fig 1](/images/2021/PRN59/1.png)

## 核心

提出一种层级式的图推理模型 (Hierarchical Graph Reasoning model, HGR)，将视频-文本匹配分解为三级语义：  

+ 全局事件，在文本中对应整个句子
+ 局部动作，在文本中对应动词
+ 实体 (entities)，在文本中对应名词短语

![Fig 2](/images/2021/PRN59/2.png)

## 层级化文本编码

### 语义角色图结构 (Semantic Role Graph Structure)

#### 构建三类节点
+ 全局事件节点
+ 动作节点
+ 实体节点

所有动作节点与全局事件节点之间构建无向边，动作与实体构建有向边。  

### 初始的图节点表征

Bi-LSTM 双向均值得到 \\(w_{i}\\)，进而通过注意力聚合得到全局特征：  

$$g_{e} = \sum_{i=1}^N \alpha_{e, i}w_{i},$$
$$\alpha_{e, i} = \frac{exp(W_{e}w_{i})}{\sum_{j=1}^N exp(W_{e}w_{j})}.$$  

对于动作与实体节点 \\(g_{a}\\) 与 \\(g_{o}\\)，利用上下文信息缓解分词错误，对 Bi-LSTM 得到的表征在需要的词上做最大值池化。  

### 基于注意力的图推理

不同级别节点间的连接能够解释局部节点与全局节点的关系，并且能够为节点消歧。  

#### 将 GCN 多关联权重分为两部分
+ 公共变换矩阵 \\(W_{t} \in \mathbb{R}^{D \times D}\\) 由所有关系类型共享
+ 角色嵌入矩阵 \\(W_{r} \in \mathbb{R}^{D \times K}\\)

初始节点嵌入为 \\(g_{i} \in \\{g_{e}, g_{a}, g_{o}\\}\\)，在第一个 GCN 层与其对应语义角色相乘：  

$$g_{i}^0 = g_{i} \odot W_{r}r_{ij},$$  

\\(r_{ij}\\) 是表示节点 \\(i\\) 到 \\(j\\) 边类型的 one-hot 向量。  

采用图注意力网络强化节点表征：  

$$\widetilde{\beta}\_{ij} = (W_{a}^q g_{i}^l)^T(W_{a}^k g_{j}^l) / \sqrt{D},$$
$$\beta\_{ij} = \frac{exp(\widetilde{\beta}\_{ij})}{\sum\_{j \in \mathcal{N}\_{i}} exp(\widetilde{\beta}\_{ij})},$$
$$g_{i}^{l + 1} = g_{i}^{l} + W_{t}^{l+1} \sum_{j \in \mathcal{N}\_{i}} (\beta_{ij}g_{j}^l).$$  

最后一层输出为文本表征 \\(c_{e}, c_{a}, c_{o}\\)。  

## 层级化视频编码

因为构建层级关系比较复杂，这里构建三种独立的视频嵌入。  

$$v_{x,i} = W_{x}^v f_{i}, \quad x \in \\{e, a, o\\}.$$  

全局特征聚合方式与文本类似。  

## 视频-文本匹配

分级匹配，与传统方法类似。  
