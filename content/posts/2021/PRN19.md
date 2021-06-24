---
title: "[论文阅读笔记 -- 跨模态哈希] Graph Convolutional Network Hashing (IJCAI 2019)"
date: 2021-06-24T15:26:11+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid", "retrieval", "hashing", "graph"]
draft: false
---

# Graph Convolutional Network Hashing for Cross-Modal Retrieval (IJCAI 2019)

本文提出一种针对跨模态检索的图卷积网络哈希 (graph convolution network hashing, GCH)，由一个语义编码器、两个特征编码网络和一个基于融合模块的图卷积网络 (GCN) 构成。  

![Fig 1](/images/2021/PRN19/1.png)

## 语义编码器

受到 Teacher-Student 策略的启发，构建一个语义编码器作为教师模块，从标签提取语义信息，用于指导特征编码过程。  

编码器定义如下：  

$$g^l = G^l(l, \theta^l).$$  

其损失函数为：  

$$ min_{\theta^l} L^l = - \alpha \sum_{i,j=1}^{n}(S_{ij}\Gamma_{ij}^l - log(1 + e^{\Gamma_{ij}^l})) + \beta ||\hat{L}^l - L||_{F}^l,$$
$$\Gamma_{ij}^l = \frac{1}{2}(H_{i}^l)(H_{j}^l)^T,$$  

其中 \\(||\cdot||_{F}\\) 是 Frobenius 范数，\\(H^l\\) 是由 \\(g^l\\) 变换得到的哈希编码， \\(\hat{L}^l\\) 是预测的标签，也可以由特征得到。  

损失函数中前一项用于保持特征相似性，后一项是原始标签与目标标签之间的分类损失。  

## 特征编码网络

在语义编码器指导下，将跨模态数据编码到公共空间。  

图像特征编码函数为 CNN，文本特征编码网络为四层 FC。  

通过语义编码器的指导，语义关联信息同教师模型传到学生模型。损失函数为：  

$$ min_{\theta^\*} L^{\*} = - \alpha \sum_{i,j=1}^{n}(S_{ij}\Gamma_{ij}^{l\*} - log(1 + e^{\Gamma_{ij}^{l\*}})) + \beta ||H^b - H^{\*}||_{F}^2 + \gamma ||\hat{L}^{\*} - L||_{F}^l,$$
$$\Gamma_{ij}^{l\*} = \frac{1}{2}(H_{i}^l)(H_{j}^{\*})^T,$$  

\\(H^{\*}\\) 是从各模态预测得到的哈希编码。\\(H^b\\) 是由 GCN 生成的灯塔哈希编码 (beacon hash code)，有助于编码更有鉴别力的特征。  

### 保留语义的融合方法 (semantic-preserving fusing method)

为了进一步增强编码得到的特征，首先需要融合编码后的特征而不损失过多语义关联，本文采用自注意力机制。  

来自两个模态的特征都使用相关模态特征重新衡量。  

$$f_{r} = \frac{1}{2}(f_{s-a}^x + f_{s-a}^y),$$
$$f_{s-a}^x = f^x \times \widetilde{W},$$
$$f_{s-a}^y = f^y \times \widetilde{W},$$  

其中 \\(\widetilde{W} = norm(f^{x} \times {f^{y}}^T)\\)，\\(f^{\*}\\) 表示来自相应模态的原始特征，\\(f_{s-a}^{\*}\\) 是处理后的特征，\\(\times\\) 表示矩阵内积，\\(norm\\) 表示矩阵范数。  

## 图卷积网络

由两个特征提取网络能够得到两个 \\(N_{b} \times d\\) 的特征矩阵，使用自注意力融合机制融合然后得到 \\(f_{r}\\)。  

利用结构化相似度进一步强化 \\(f_{r}\\) 以弥补模态差异造成的损失。故此采用多层 GCN，邻接矩阵 \\(A \in \mathbb{R}^{N_{b} \times N_{b}}\\)：  

$$H^{(l+1)} = \sigma(\widetilde{D}^{-\frac{1}{2}}\widetilde{A}\widetilde{D}^{-\frac{1}{2}}H^{(l)}W^{(l)}),$$  

\\(\widetilde{A} = A + I_{N}\\) 是无向图 \\(G\\) 规范化后的邻接矩阵，\\(A(i, j) = l_{i} \times l_{j}\\), \\(\widetilde{D}\\) 是 \\(\widetilde{A}\\) 的度矩阵。  

这部分损失函数定义为：  

$$ min_{\theta^G} L^{GCN} = - \alpha \sum_{i,j=1}^{n}(S_{ij}\Gamma_{ij}^{G} - log(1 + e^{\Gamma_{ij}^{G}})) + \beta ||\hat{L}^{G} - L||_{F}^l,$$
$$\Gamma_{ij}^{G*} = \frac{1}{2}(H_{i}^b)(H_{j}^b)^T,$$  

GCN 的输出成为灯塔特征 (beacon feature)，用于在公共空间中起到灯塔的作用。  

## 应用处理

对于实际的未在训练集出现的数据点，其哈希编码通过特征提取 + 符号函数得到。  
