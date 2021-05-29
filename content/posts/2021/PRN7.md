---
title: "[论文阅读笔记 -- VQA] Beyond RNNs: Positional Self-Attention with Co-Attention(AAAI 2019)"
date: 2021-05-29T14:57:33+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Beyond RNNs: Positional Self-Attention with Co-Attention for Video Question Answering (AAAI 2019)

## RNN + Attention 方法问题
+ 耗时
+ 由于 RNN 的特点，难以建模长距离依赖关系

本文提出了一个名为 Positional Self-Attention with Co-attention (PSAC) 的新架构，是首个无需使用 RNN 的模型。  

![Fig 1](/images/2021/PRN7/1.png)

## Positional Self-Attention Block
取代 RNN 以更好捕捉长距离依赖关系和位置信息。  

将序列化的特征 \\(F \in \mathbb{R}^{n \times d_{}}\\)同时视作 query、key 和 value，采用如下的 scaled dot product attention:  

\\[SPDA(F^Q, F^K, F^V) = softmax(\frac{F^K(F^Q)^T}{\sqrt{d_k}}) F^V.\\]

而后再加上跳连和 layer normalization。

自注意力能够确保计算效率并建模长距离依赖关系，但是忽略了输入序列的位置信息。为了弥补这一缺陷，本文定义了一个位置矩阵 \\(P \in \mathbb{R}^{n \times d_{k}}\\)，对 \\(F\\) 的位置信息进行编码:

\\[P_{pos, 2j} = sin(pos/10000^{2j/d_{k}}),\\]

\\[P_{pos, 2j+1} = cos(pos/10000^{2j/d_{k}}).\\]

最终特征由 P 和 layer normalization 输出相加后经两个卷积层和一个中间的 ReLU 层处理得到。  

由上述方法可直接得到视频的位置自注意力特征，文本方面则可按两种层级描述信息：word + character，C 卷积后与 W 拼接得到问题编码，进而用 depthwise 卷及层进一步融合，最后得到位置自注意力特征。  

其中各个单词得到 300-D 向量，各个字符得到 64-D 向量。  

## Video-Question Co-Attention Layer
两个方向，与 Co-attention 类似。  
