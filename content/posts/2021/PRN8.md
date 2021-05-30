---
title: "[论文阅读笔记 -- VQA] Location-Aware Graph Convolutional Networks (AAAI 2020)"
date: 2021-05-29T18:39:03+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Location-aware graph convolutional networks for video question answering (AAAI 2020)

## 与 IQA 相比 VQA 的几个困难
+ 由于视频包含大量帧，其视觉内容更为复杂，尤其是其中一些帧主要被与问题无关的背景内容 (strong background content) 占据
+ 视频通常包含多种动作，但只有其中一部分是问题所关心的
+ 需要关注目标的时域位置 (temporal location) (如 before / after) 以及其相互之间的复杂关系

![Fig 1](/images/2021/PRN8/1.png)

## 现有方法的问题
+ 现有的 spatio-temporal 注意力机制通常不够健壮，容易受到视频中复杂背景内容的影响
+ 而通过对个帧做目标检测并用 LSTM 进行序列化处理时输入目标序列的顺序会影响表现，二者通常很难排序；此外通过递归方式处理目标难免会忽略非邻接目标之间的直接关联

![Fig 2](/images/2021/PRN8/2.png)

本文提出 Location-aware Graph Convolutional Networks (L-GCN) 对与问题相关的目标之间的相互关系进行建模。  

## 问题编码器
为了处理词典外的单词和拼写错误的单词，将单词级别嵌入与字符级别嵌入结合得到问题特征，实现时，单词嵌入函数采用预训练的 300-D GloVe，字符嵌入函数随机初始化。  

问题特征通过两层的 highway 网络得到：  

\\[Q = h(Q^w, g(Q^c)),\\]

其中字符嵌入首先由一个 2D 卷积层 \\(g(\cdot)\\) 处理。为了对问题进行更好的编码，将 \\(Q\\) 传入 bi-LSTM 进一步处理，最后通过堆叠 bi-LSTM 每个方向上各个时间步的隐状态得到问题特征 \\(F^Q\\)。  

## Location-aware 图的构建

由于动作可以由目标的相互关系推断而来，本节对检测到的目标构建一个全连接图 (fully-connected graph)。  

可以用目标特征表示各个节点，但这样的节点类型会忽视目标的位置信息，而位置信息对于与时域关系相关问题的推理至关重要。  

故此，将位置信息编码为所谓的位置特征 (location features)，从而能够构建起一个 location-aware 图，即将目标的外观特征与位置特征拼接作为节点特征。  

### 位置编码

对于从一个从第 n 帧检测到的一个目标，其空间位置 \\(b\\) 表示为 top-left 坐标以及检测到目标的宽和高，其 aligned 特征表示为 \\(o\\)，对其用 MLP 进行编码得到空间位置特征  

\\[d^s = MLP(b),\\]

此外，用正弦和余弦对其时域位置特征进行编码  

\\[d^t_{2j} = sin(n/10000^{2j/d_p}),\\]

\\[d^t_{2j+1} = cos(n/10000^{2j/d_p}),\\]

\\(d_p\\) 是其维度。接着拼接得到节点特征  

\\[v = \[o; d^s; d^t\].\\]

通过这种方式，图中的各个节点不仅包含目标的外观特征，也包含位置特征。  

## 通过图卷积进行推理

构建 P 层图卷积网络以得到区域特征 (regional features)，对于第 p 层 (\\(1 \le p \le P\\))：  

\\[X^{(p)} = A^{(p)}X^{(p-1)}W^{(p)},\\]

其中 \\(A^{(p)}\\) 是通过第 p 层节点特征运算而来的邻接矩阵：  

\\[A^{(p)} = softmax(X^{(p-1)}W_{1} \cdot (X^{(p-1)}W_{2})^{T}),\\]

softmax 沿着行计算，最终通过跳连得到区域特征：  

\\[F^R = X^{(P)} + X^{(0)}.\\]

本文 \\(P = 2\\)。  

## 视觉编码器

用如 ResNet-152 的固定特征提取器提取帧特征，为了引入上下文信息，对帧特征集合使用全局均值池化，得到全局特征 \\(F^G\\)i，进而由 1D 卷积 + ELU 处理以合并来自邻帧的信息。

每帧用 K 个区域框进行目标检测，本文使用 RoIAlign (Mask R-CNN) + FC + ELU。对检测到的目标集合构建 location-aware 图，进而进行图卷积，得到区域特征 \\(F^R\\)。

而后，将 \\(F^G\\) 重复 K 次后与 \\(F^R\\) 拼接，用 MLP 处理得到视觉特征：  

\\[F^V = MLP(\[F^R, F^G\]).\\]

本文 \\(K = 5\\)。  

## 视觉-问题交互模块 (Visual-question Interaction Module)

首先用两个独立的 FC 将视觉与问题特征转化到同一维度，后续就是较为常规的注意力机制 + bi-LSTM + 最大值池化。  

## 答案推断与损失函数
与以前的文章类似。  
