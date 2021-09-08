---
title: "[论文阅读笔记 -- 跨模态检索] Learning Sufficient Scene Representation(Neurocomputing 2021)"
date: 2021-09-08T19:36:34+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "unsupervised"]
draft: false
---

# Learning Sufficient Scene Representation for Unsupervised Cross-modal Retrieval (Neurocomputing 2021)

![Fig 1](/images/2021/PRN91/1.png)

## 背景

此前有工作从统计层面证明分析了跨模态检索的过程，借助变分推断证明了不可能同时最大化模态内与模态间相似度，二者会互相约束。  

对应的图文数据可以看作相同场景在不同视角下的描述，二者以互补形式对场景进行描述，因而二者之间含有共现信息 (co-existed information) 与互补信息 (complementary information)。  

本文提出一种基于充分场景表征的无监督跨模态检索方法 (unsupervised cross-modal retrieval method via sufficient scene representation, CMSSR)

单模态流形 (manifolds) 可以被视为高维流形的子流形。  

CMSSR 为各多模态数据学习一个充分的场景表征，在场景级别衡量其相似度，而不是像现有方法一样只是关注公共空间中的相似度关系。  

![Fig 2](/images/2021/PRN91/2.png)

## 从单模态数据学习统计表征

由于数据特征，通常可以从文本提取高阶语义特征，而只能从图像提取低阶视觉特征。  

为了缩小语义鸿沟，采用统计方法进一步抽象低阶图像特征，并模糊高阶文本语义，将二者嵌入到一致的统计层级。  

本文在此处采用了混合高斯模型 (GMM)。  

### 基于 GMM 的文本表征

从单词层面开始着手，基于 GMM 建模单词 \\(W = \\{x_{i}\\}\_{i=1}^{N_{W}}\\) 的分布：  

$$p(x|\pi^{T}) = \sum_{i=1}^{K_{T}} \pi_{i}^{T} g_{i}^{T},$$

其中 \\(g_{i}^{T} = N(x | \mu_{i}^{T}, \sigma_{i}^T)\\) 是第 \\(i\\) 个高斯密度函数，\\(\pi_{i}^{T}\\) 是其权重。  

\\(K_{T}\\) 个函数构成函数集合 \\(\mathbb{G}^{T}\\)，用其将文本数据 \\(X\\) 表征为  

$$p(x|X, \xi) = \sum_{i=1}^{K_{T}} \xi_{i} g_{i}^{T},$$
$$\xi_{i} = \frac{\sum_{x \in X} \pi_{i}^{T} g_{i}^{T}}{\sum_{j=1}^{K_{T}} \sum_{x \in X} \pi_{j}^{T} g_{j}^{T}}, i = 1, 2, \cdots, K_{T}.$$

### 基于 GMM 的图像表征

提取局部特征后聚类得到一组 visual words，进而过程类似。  

$$p(y|\pi^{I}) = \sum_{i=1}^{K_{I}} \pi_{i}^{I} g_{i}^{I},$$
$$p(y|Y, \eta) = \sum_{i=1}^{K_{I}} \eta_{i} g_{i}^{I},$$
$$\eta_{i} = \frac{\sum_{y \in Y} \pi_{i}^{I} g_{i}^{I}}{\sum_{j=1}^{K_{I}} \sum_{y \in Y} \pi_{j}^{I} g_{j}^{I}}, i = 1, 2, \cdots, K_{I}.$$

### 基函数求取

用 EM 算法估计 GMM 的参数。  

![Alg 1](/images/2021/PRN91/A1.png)

## 单模态统计流形与公共统计流形

图文数据分别被嵌入到由 \\(\mathbb{G}^{I}\\) 和 \\(\mathbb{G}^{T}\\) 张成的流形 \\(\mathcal{M}^{I}\\) 和 \\(\mathcal{M}^{T}\\) 中。  

由于 \\(\mathbb{G}^{I}\\) 和 \\(\mathbb{G}^{T}\\) 线性无关，拼接二者构建公共统计流形 \\(\mathcal{M}^{C}\\)，各场景对应其中的一个点。

### 距离度量

对于一个混合族流形 \\(\mathcal{M}\\) 中的两个数据点 \\(p = p(w | \theta^{p})\\) 与 \\(q = p(w | \theta^{q})\\)，通常用 \\(f\\) 散度计算其不相似性：  

$$D_{f}[p : q] = \sum \theta_{i}^{p} f(\frac{\theta_{i}^q}{\theta_{i}^p}),$$

其中 \\(f(\cdot)\\) 是满足 \\(f(1) = 0\\) 的凸函数。  

计算散度需要大量采样，为解决这一问题，将统计流形中各数据视为一个函数，直接计算两个数据点之间的函数距离。  

可将 2 范数函数距离变换为测地线 (geodesic) 距离，最终写作：  

$$D(p, q) = \sqrt{(\theta^p - \theta^q)^T \phi (\theta^p - \theta^q)} = ||\theta^p - \theta^q||\_{2}.$$

## Representation Completion in Statistical Manifold for Cross-modal Retrieval

数据在公共统计流形中的表征并不完整，为 \\((\xi_{1}, \cdots, \xi_{K_{T}}, 0)\\) 或 \\((0, \eta_{1}, \cdots, \eta_{K_{I}})\\)，需要补充完整。  

通过将数据投影到另一模态的统计流形中，并用另一组基函数进行表征实现这一目的。  

基于逻辑斯蒂回归学习两中投影。  

$$\hat{\eta} = P^{T \rightarrow I}(\xi) = Softmax(\omega^T \xi),$$
$$\hat{\xi} = P^{I \rightarrow T}(\eta) = Softmax(\varphi^T \eta),$$
$$loss^{T \rightarrow I} = min_{\omega} ||\eta - \hat{\eta}||\_{2} + \alpha ||\omega||\_{1},$$
$$loss^{I \rightarrow T} = min_{\varphi} ||\xi - \hat{\xi}||\_{2} + \beta ||\varphi||\_{1},$$

具体算法如下：  

![Alg 2](/images/2021/PRN91/A2.png)

## 生成充分图表征的二值编码

使用迭代量化 (ITQ) 学习能够保留相似度的二值编码，学习一个作用于数据的旋转矩阵，最小化量化损失，生成二值编码。  

具体算法如下：  

![Alg 3](/images/2021/PRN91/A3.png)

得到旋转矩阵 \\(R\\) 以后，对于 query 数据 \\(\psi\\)，首先由预训练的 PCA 求得低维表征 \\(h \in \mathbb{R}^{1 \times L}\\)，进而计算二值编码：  

$$b = sgn(hR).$$

## 流形可视化示例

![Fig 10](/images/2021/PRN91/10.png)

## 基函数个数分析

![Fig 11](/images/2021/PRN91/11.png)
