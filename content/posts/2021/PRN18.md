---
title: "[论文阅读笔记 -- 跨模态检索] Cross-modal Scene Graph Matching (WACV 2020)"
date: 2021-06-23T09:36:01+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid", "retrieval"]
draft: false
---

# Cross-modal Scene Graph Matching for Relationship-aware Image-Text Retrieval (WACV 2020)

![Fig 1](/images/2021/PRN18/1.png)

## 出发点

正确的匹配除了要包含相同的目标以外，目标之间的关系也应当相同。  

因而，本文使用视觉场景图 (visual scene graph, VSG) 和文本场景图 (textual scene graph, TSG) 来表征图像与文本数据，将传统的图文检索问题转化为了场景图匹配问题。  

整体上设计了一个场景图匹配 (Scene Graph Matching, SGM) 模型。  

图像首先被表征为 VSG，进而编码为视觉特征图 (visual feature graph, VFG)；文本首先被i表征为 TSG，进而编码为文本特征图 (textual feature graph, TFG)。  

最终，模型从 VFG 和 TFG 收集目标特征和关系特征，在目标级别和关系级别计算相似度分数。  

![Fig 2](/images/2021/PRN18/2.png)

## 视觉特征嵌入

### 视觉场景图 (VSG) 的生成

用现有的场景图生成模型 (如 MSDN 和 Neural Motifs) 生成 VSG。  

VSG 中包含两种节点，一种是目标节点，对应图像中的某个区域；另一种是关系节点，是连接两个目标节点的有向边。各个节点都有一个类标签 (如 man、hold)。  

![Fig 3](/images/2021/PRN18/3.png)

### 视觉场景图编码器

设计了一种多模态图卷积网络 (Multi-modal Graph Convolutional Network, MGCN) 来生成 VSG，包含如下几个部件。  

#### 视觉特征提取器

用与训练的 CNN 或目标检测器提取特征，将各个节点编码为 \\(d_{1}\\) 维视觉特征向量。  

目标节点用对应的图像区域编码，关系节点用所涉及的两个图像区域的并集编码。  

本文选用了在 Visual Genome 数据集上预训练的 Faster-RCNN，使用 RoI 池化后的 2048 维特征向量。  

#### 标签嵌入层

各节点都有一个由视觉场景图生成器所赋予的单词标签，能够提供辅助的语义信息，呈现为 one-hot 形式。  

对其进行线性变换，其参数用 word2vec 初始化，维度设为 \\(d_{2} = 300\\)。  

#### 多模态融合层

将节点特征与其标签特征拼接后做一次非线性变换以融合为一个 \\(d_{1}\\) 维的特征，激活函数采用 tanh。  

#### 图卷积网络

采用 \\(m\\) 层 GCN 编码多模态的融合特征图，并提出一种新的更新机制，以不同的方式更新两种节点。  

目标节点可以生成目标级别的特征，因此视作一阶图像特征。其他目标节点或关系节点的信息可能会损坏一个目标节点的信息，因此各目标节点不使用其他来自邻居的信息进行更新。  

关系节点是二阶图像特征，所以其特征能通过邻接目标节点得到增强。  

每层 GCN 由 FC + tanh 构成，目标节点以自身为输入，关系节点以自身以及两个邻接的目标节点为输入。  

实际应用中本文 \\(m = 1\\)。  

![Fig 4](/images/2021/PRN18/4.png)

## 文本特征嵌入

### 文本场景图 (TSG) 的生成

TSG 中包含两种边，黑色箭头表示单词顺序边 (word-order edges)，按照句中顺序连接单词；棕色箭头表示语义关系边 (semantic relationship edges)，由 SPICE 通过语义三元组解析得到 (如 man-hold-baby)。  

根据不同的边，在图中形成不同种类的路径，单词顺序路径 (word-order path) 和语义关系路径 (semantic relationship path)。  

### 文本场景图编码器

从 TSG 提取目标特征和关系特征，由词嵌入层、单词级别的 bi-GRU 和路径级别的 bi-GRU 组成。  

单词级别 bi-GRU 沿着单词顺序路径编码各个节点，编码后带上下文的目标级别特征在各隐状态生成。  

路径级别 bi-GRU 沿着语义关系路径编码后得到显式的关系级别特征。  

最终的各个特征均为双向隐状态的均值。  

## 相似度函数

分开匹配两种级别的特征。  

用内积计算两两之间的相似度，并为每个文本目标找到相似度最大的视觉目标，保留其相似度，对这些相似度求均值，得到最终的分数。

最后将两种级别的相似度分数相加。  

## 损失函数

对相似度分数使用三元组损失。  
