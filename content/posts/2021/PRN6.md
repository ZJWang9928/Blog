---
title: "[论文阅读笔记 -- 跨模态检索] Consensus-Aware Visual-Semantic Embedding (ECCV 2020)"
date: 2021-05-29T10:55:05+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal retrieval", "cross-modal"]
draft: false
---

# 2007.08883 Consensus-Aware Visual-Semantic Embedding for Image-Text Matching (ECCV 2020)

![Fig 1](/images/2021/PRN6/1.png)

## 当前主流方法
+ 将图像与文本投影到一个公共空间，通常无法充分利用图像中目标以及句子段之间的关系
+ 分块级别的匹配 (fragment-level matching) + 聚合其相似度以衡量关联度，但通常只利用匹配图文对，本文称之为实例级别的对齐 (instance-level alignment)


## 本文观点
人类除了能利用配对信息以外，还能利用常识知识。本文通过目标的共同出现几率构建常识 (consensus)，在 instance-level 特征之外学习 consensus-level 的特征，提出 Consensus-aware Visual-Semantic Embedding (CVSE) 架构。  

![Fig 2](/images/2021/PRN6/2.png)

## Consensus Exploitation (CE) Module

### 概念实例化 (Concept Instantiation)

将文本标注语料中的句子用于提取常识知识，将其表示为语义概念及其之间的关联。  

由于单词库较大且存在一些没有意义的单词，将较少出现的单词除去。具体实现时，选取 top-q 出现频率的单词，并将其粗略分为三类：  

+ 目标 (Object)  
+ 动作 (Motion)  
+ 属性 (Property)  

三类概念的比例设为 (7:2:1)，进而用 GloVe 对所选概念进行实例化。  

### 概念相关性图构建 (Concept Correlation Graph Building)

构建一个条件概率矩阵 \\(P\\) 对不同概念之间的相关性进行建模，各元素 \\(P_{ij}\\) 表示当概念 \\(C_{i}\\) 出现时概念 \\(C_{j}\\) 出现的概率

\\[P_{ij} = \frac{E_{ij}}{N_{i}},\\]

其中 \\(E \in \mathbb{R}^{q \times q}\\) 是概念共现矩阵，其元素表示两个概念的共同出现次数，\\(N_{i}\\) 表示 \\(C_{i}\\) 在语料中的出现次数。  

值得注意的一点是，\\(P\\) 是非对称矩阵，这使得其能够捕捉到不同概念之间合理的相互依赖性，而非简单统计贡献频数。  

#### P 矩阵的几个缺陷
+ 由于是由标注语料构建的，可能会偏离真实场景下的数据分布，进而削弱泛化能力
+ 有共现频率得到的统计模式容易受到长尾分布的影响，导致相关性图有偏差

为了解决上述问题，本文设计了一个 Confidence Scaling (CS) 函数，用于调整 \\(P\\) 矩阵的尺度：  

\\[B_{ij} = f_{CS}(P_{ij}) = s^{P_{ij}-u} - s^{-u},\\]

其中 s 和 u 是两个预定义的参数，用于决定 \\(P\\) 中元素调整尺度时扩张或收缩的比率。接着，为了避免相关性矩阵过拟合于训练数据并提升其泛化能力，对 B 进行二值化操作：  

\\[G_{ij} = \begin{cases} 0, \ if \ B_{ij} < \epsilon, \\\\ 1, \ if \ B_{ij} \ge \epsilon, \end{cases}\\]

### Consensus-Aware Concept Representation
使用多层堆叠的 GCN 层 (CGCN module) 学习概念特征，从而在概念之间引入高次的近邻信息 (neighborhoods information)以建模其互相之间的依赖关系。  

取 GCN 最后一层的输出作为最终的概念特征 \\(Z \in \mathbb{R}^{q \times d}\\)，\\(z_{i}\\) 表示概念 \\(C_{i}\\) 的 d 维特征，称 \\(Z\\) 为 consensus-aware concept (CAC) 特征。  

## Consensus-Aware Representation Learning

![Fig 3](/images/2021/PRN6/3.png)

用上一节得到的常识生成图像与文本的 consensus-aware 特征。  

### 实例级别图文特征
用预训练的 Faster-RCNN + FC 提取视觉特征，bi-GRU + 平均池化提取文本特征。接着用自注意力机制强化局部特征，其中视觉全局特征由视觉局部特征取均值得到。  

### 常识级别图文特征
为了利用常识信息，将实例级别图文特征(\\(v^{I}\\) 和 \\(t^{I}\\))作为输入在 CAC 特征中进行查询 (query)。  

#### 视觉概念级别特征

\\[a_{i}^{v} = \frac{exp(\lambda v^{I}W^{v}z_{i}^{T})}{\sum_{i=1}^{q}exp(\lambda v^{I}W^{v}z_{i}^{T})},\\]

\\[v^{C} = \sum_{i=1}^{q}a_{i}^{v} \cdot z_{i},\\]

#### 文本概念级别特征
由于语义概念是从文本数据实例化得来的，可以将出现在相应文本描述中的一组概念作为该本文描述的标签，称之为概念标签 (concept label) \\(L^{t} \in \mathbb{R}^{q \times 1}.\\)

\\[a_{i}^{t} = \alpha \frac{exp(\lambda L_{j}^{t})}{\sum_{j=1}^{q}exp(\lambda L_{j}^{t})} + (1 - \alpha) \frac{exp(\lambda t^{I}W^{t}z_{j}^{T})}{\sum_{j=1}^{q}exp(\lambda t^{I}W^{t}z_{j}^{T})},\\]

\\[t^{C} = \sum_{j=1}^{q}a_{j}^{t} \cdot z_{j}.\\]

## 对常识级别与实例级别特征进行融合

\\[v^F = \beta v^I + (1 - \beta) v^C,\\]

\\[t^F = \beta t^I + (1 - \beta) t^C.\\]

## 训练
+ FF、CC、II 之间使用 triplet ranking loss
+ 对概念分数使用 KL 散度

\\[D_{KL}(a^t || a^v) = \sum_{i=1}^q a_{i}^{t} log(\frac{a_{i}^{t}}{a_{i}^{v}}).\\]

##推断
计算 \\(v^F\\) 和 \\(t^F\\) 之间余弦相似度。  

使用一种概念预测策略来缩小训练与推断阶段的差异。  

+ Text-to-text similarity
+ Cross-modal similarity
