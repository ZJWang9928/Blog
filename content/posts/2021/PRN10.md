---
title: "[论文阅读笔记 -- VQA] Reasoning with Heterogeneous Graph Alignment (AAAI 2020)"
date: 2021-06-03T18:30:46+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Reasoning with Heterogeneous Graph Alignment for Video Question Answering

![Fig 1](/images/2021/PRN10/1.png)

## 出发点

需要一个统一的方法同步对模态间和模态内的关联性进行建模与推理。  

![Fig 2](/images/2021/PRN10/2.png)

本文所提到的 “视频段 (video shot)” 指的是一小段能被 3D 卷积模块处理并产生一个动作向量 (motion vector) 的视频片段。  

## 视频与文本上下文特征

### 视觉模态

由于视频段比帧级别具有更丰富的表达能力，因此使用 3D 卷积网络 C3D 处理得到视频段级别的视频动作特征；同时使用 2D 卷积网络 ResNet 提取帧级别特征作为补充。从而得到外观特征组 \\(F_{A}\\) 和动作特征组 \\(F_{M}\\)，各包含 \\(L_{v}\\) 个特征， \\(L_{v}\\) 是视频的帧数，拼接后用两个 FC 层将其投影到一个公共视觉空间之中，得到特征组 \\(F\\)，也包含 \\(L_{v}\\) 个特征。  

### 文本模态

GloVe 进行词嵌入得到 \\(E\\)，句子长度为 \\(L_{q}\\)。  

### 聚合动态时域信息得到上下文特征

用两个独立的 GRU 分别处理视觉与文本模态的特征组 \\(F\\) 和 \\(E\\)，得到最终的视频特征 \\(V \in \mathbb{R}^{L_{v} \times d}\\) 和问题特征 \\(Q \in \mathbb{R}^{L_{q} \times d}\\)。  

## 跨模态联合融合与对齐 (Cross-Modal Joint Fusion and Alignment)
接着利用一个模块化的联合注意力嵌入操作 (co-attention embedding operation, CAEO) 将特征嵌入到一个公共空间之中。根据 scaled dot-product self-attention 定义一个通用的 CAEO:  

\\[CAEO(\mathbb{M}_{Q}, \mathbb{M} _{K}, \mathbb{M} _{V}) = softmax(\frac{\mathbb{M} _{Q} \mathbb{M} _{K}^{T}}{\sqrt{d}}) \mathbb{M} _{V},\\]

在多数情况下，\\(\mathbb{M} _{Q}\\) 和 \\(\mathbb{M} _{K}\\) 通常表示两种不同的模态，而 \\(\mathbb{M} _{K}\\) 和 \\(\mathbb{M} _{V}\\) 通常相同。  

接着本文提出了一系列 CAEO 的变体，如图 3 所示。都加上了残差连接，模块输出统一标记为 \\(Q _{update}\\) 和 \\(V _{update}\\)。  

![Fig 3](/images/2021/PRN10/3.png)

### 语言空间到视觉空间的变换

\\[Q _{CAEO} = CAEO(W _{q}^q Q, W _{k}^q V, W _{v}^q V),\\]

\\[Q _{q \rightarrow v} = LayerNorm(FF _{q}(Q _{CAEO}) + Q).\\]

其中 \\(FF _{q}\\) 是前向传播 (feed-forward) 模块，实现为线性变换。得到的特征称为 Linguistic Visual。  

### 视觉空间到语言空间的变换

\\[V _{CAEO} = CAEO(W _{q}^v V, W _{k}^v Q, W _{v}^v Q),\\]

\\[V _{v \rightarrow q} = LayerNorm(FF _{v}(V _{CAEO}) + V).\\]

与上一节类似，得到的特征称为 Visual Linguistic。  

### 联合注意力变换

交叉变换对于融合与对齐跨模态信息至关重要，因此整合前两节的变换，引入对称的联合注意力操作，并称之为 CoAtten。  

### 伪孪生联合注意力 (pseudo-siamese co-attention) 变换

用前面描述的方法得到 \\(Q _{CAEO}\\) 和 \\(V _{CAEO}\\)，并如下得到联合注意力输出：  

\\[Q _{q \rightarrow v} = LayerNorm(FF _{q}(\[Q _{CAEO}; Q\]) + Q),\\]

\\[V _{v \rightarrow q} = LayerNorm(FF _{v}(\[V _{CAEO}; V\]) + V).\\]

### 孪生联合注意力 (siamese co-attention) 变换

与伪孪生类似，唯一的区别在于采用了共享的前向传播模块。  

## 异构图推理 (Heterogeneous Graph Reasoning)

以各视频段和问题中各个单词作为节点，构建无向异构图 (undirected heterogeneous)。拼接得到包含全部视觉与文本向量的异构输入矩阵 \\(X\\)：  

\\[X = \[Q _{update}, V _{update}\]^T.\\]

将 \\(X\\) 中的向量作为节点。  

### 跨模态对齐邻接矩阵

非线性变换 + 内积得到节点之间的相关性分数：  

\\[G = \phi(X)\phi(X)^{T},\\]

\\(G\\) 各行用 softmax 归一化后被用作邻接矩阵。  

### 在异构图上推理

用 GCN 进行推理  

\\[Z = GXW,\\]

对 Z 使用自注意力池化，得到一个局部结果向量 (local result vector) \\(s _{local}\\)，自注意力池化定义为 \\(\rho := (FC(d)-Tanh-FC(1)-Softmax)\\)：  

\\[s _{local} = \sum^{N} \rho(Z)^TZ.\\]

## 全局与局部信息融合

由于图结构的推理模块更多关注局部因子之间的交互，可能会损失全局的语义信息。因此使用双线性模块融合全局与局部信息。  

首先融合 GRU 的最后隐状态得到全局特征：  

\\[s _{global} = Bilinear(q _{L _{q}}, v _{L _{v}}).\\]

接着融合全局与局部信息得到最终的输出：  

\\[s = Bilinear(s _{global}, s _{local}).\\]

## 答案预测与评估
与前文类似。  
