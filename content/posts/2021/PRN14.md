---
title: "[论文阅读笔记 -- 跨模态检索] AXM-Net: Cross-Modal Context Sharing Attention Network (2021)"
date: 2021-06-21T14:24:22+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid"]
draft: false
---

# 2101.08238 AXM-Net: Cross-Modal Context Sharing Attention Network for Person Re-ID (2021)

![Fig 1](/images/2021/PRN14/1.png)

## 主要困难
+ 各模态中与行人相关的信息结构相当不同
+ 关键在于学习一个能够从数据中提取语义的网络，而不是在训练过程中简单记住各行人相应的图文对
+ 特征中所表征的语义信息，如衣着的颜色与类型、行人动作等，应当在模态间被对齐，且不受背景噪音干扰

![Fig 2](/images/2021/PRN14/2.png)

## 视觉与文本特征学习网络

### 一个想法
学习多尺度特征，基于语义信息跨模态对齐特征。  

故此，提出一种图文之间的自适应的上下文共享语义对齐模块 (adaptive context sharing semantic alignment block, AXM-block)，如图 2 所示，各个块由从不同感受野 (receptive fields, R) 获得多模态特征并捕捉不同数量局部上下文的多个特征流与一个上下文共享语义对齐网络 (context sharing semantic alignment network) 组成。  

AXM-block 的输入具有丰富的模态内语义，有来自不同上下文支持区域 R 的多粒度信息。  

![Tab 1](/images/2021/PRN14/T1.png)

特征学习骨架网络如表 1 所示。  

![Fig 3](/images/2021/PRN14/3.png)

### 自适应的跨模态上下文共享语义对齐网络 (Adaptive Cross-modal Context Sharing Semantic Alignment Networ)

直觉上，各模态中的语义信息应当是相同的，因为其描述的是同一个行人。故此，提出该网络来动态地结合这些信息。  

网络通过更加注意各模态中的显著语义信息，并同时减小无用部分信息的影响，来增强语义信息。由于文本输入没有背景信息， AXM-Net 能够减少背景所造成的影响。  

定义第 \\(r\\) 个感受野的特征图组为 \\(X = (V_{r}, T_{r})\\)，对特征图使用 GAP 后得到 \\(x = (v_{r}, t_{r})\\)，将这些向量传入上下文与语义学习迷你网络 (context and semantic learning mini-network) 中得到缩放向量 (scale vectors) 基于跨模态语义对信息进行缩放：  

$$\widetilde{x} = C(x_{r}) \quad \forall v_{r}, t_{r}\ in\ x$$

函数 \\(C(x)\\) 表示非线性映射。进而进行特征对齐 (feature alignment)：  

$$(\widetilde{V}, \widetilde{T}) = \sum_{r=1}^{R}(V_{r} \odot \widetilde{v_{r}}, T_{r} \odot \widetilde{t_{r}}).$$

![Fig 4](/images/2021/PRN14/4.png)

## 视觉模态的注意力局部特征学习

全局视觉分支关注行人整体特征，但是模态内的局部信息同样重要。  

因此提出一种基于部分的注意力网络 (part based attention network) 来增强各局部视觉部分中的信息，对各个局部块做通道注意力。为了在空间区域之间使用上下文信息，局部注意力部分的参数共享。  

![Fig 5](/images/2021/PRN14/5.png)

## 文本特征的非局部注意力

使用非局部注意力 (non-local attention, NLA) 通过直接计算一对文本特征之间的相互关系对文本中的长距离依赖关系进行建模。  

## 目标函数

ID Loss + Triplet Loss + Affinity Loss  

## Cross-modal Affinity Loss

基于图文对之间的关联性，实现为二元交叉熵 (binary cross entropy)：  

$$L_{aff} = -[y_{ij} \cdot log(\sigma(S(V_{i}, T_{j}))) + (1 - y_{ij}) \cdot log(\sigma(1 - S(V_{i}, T_{j})))],$$  

配对时 \\(y_{ij} = 1\\)，否则 \\(y_{ij} = 0\\)。

![Fig 6](/images/2021/PRN14/6.png)
