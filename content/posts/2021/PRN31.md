---
title: "[论文阅读笔记 -- 跨模态检索 / 图网络] Similarity Reasoning and Filtration (2021)"
date: 2021-07-03T21:00:55+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "graph", "retrieval"]
draft: false
---

# 2101.01368 Similarity Reasoning and Filtration for Image-Text Matching (2021)

[开源代码传送门](https://github.com/Paranioar/SGRAF)

![Fig 1](/images/2021/PRN31/1.png)

## 先前方法的缺陷

+ 在局部特征之间计算基于标量的余弦相似度，可能并不足以表征区域与单词之间的关联模式
+ 大多数方法使用池化去聚合区域与单词之间所有的潜在对齐 (all the latent alignments between regions and words)，可能会阻碍局部与全局对齐之间的信息交流
+ 无法排除无意义特征所带来的干扰

本文设计了一种相似度图推理与注意力过滤网络 (Similarity Graph Reasoning and Attention Filtration network, SGRAF)。  

为了更高效地建模跨模态关联，学习基于向量的相似度表征，而非基于标量的余弦相似度。  

![Fig 2](/images/2021/PRN31/2.png)

## 特征提取

### 视觉特征提取

Faster R-CNN  
自注意力机制利用所有局部特征求得全局特征。  

### 文本特征提取

Bi-GRU  
自注意力机制利用所有局部特征求得全局特征。  

## 相似度表征学习

### 向量相似度函数

两个 \\(d\\) 维向量 \\(x\\) 和 \\(y\\) 之间的相似度定义为：  

$$s(x, y; W) = \frac{W|x - y|^2}{||W|x - y|^2||\_{2}} \in \mathbb{R}^{m}.$$

### 全局相似度表征

$$s^g = s(\overline{x}, \overline{y}; W_{g}).$$  

### 局部相似度表征

首先用 textual-to-visual 注意力根据各单词处理各个区域，得到 \\(a_{j}^v\\)，进而有  

$$s_{j}^l = s(a_{j}^v, t_{j}; W_{l}).$$  

## 相似度图推理 (Similarity Graph Reasoning, SGR)

### 图的构建

构建相似度图在可能的局部与全局特征之间传播相似度信息。  

图节点 \\(\mathcal{N} = \\{s_{1}^l, \cdots, s_{L}^l, s^g\\}\\)，而节点 \\(s_{p}\\) 与 \\(s_{q}\\) 之间的有向边为  

$$e(s_{p}, s_{q}; W_{in}, W_{out}) = \frac{exp((W_{in}s_{p})(W_{out}s_{q}))}{\sum_{q}exp((W_{in}s_{p})(W_{out}s_{q}))}.$$

### 图推理

如下更新  

$$\hat{s}\_{p}^n = \sum_{q}e(s_{p}^n, s_{q}^n; W_{in}^n, W_{out}^n) \cdot s_{q}^n,$$
$$s_{p}^{n+1} = ReLU(W_{r}^n \hat{s}\_{p}^n)e(s_{p}, s_{q}; W_{in}, W_{out}).$$

最终取最后一步迭代的全局特征，过一层 FC 后得到最终的相似度分数。  

## 相似度注意力过滤 (Similarity Attention Filtration, SAF)

排除无意义的特征。  

对于局部与全局相似度表征，为每个相似度表征计算一个聚合权重：  

$$\beta_{p} = \frac{\sigma(BN(W_{f}s_{p}))}{\sum_{s_{q} \in \mathcal{N}} \sigma(BN(W_{f}s_{q}))},$$
$$W_{f} \in \mathbb{R}^{m \times 1}.$$  

接着就是特征聚合：  

$$s_{f} = \sum_{s_{p} \in \mathcal{N}} \beta_{p}s_{p}.$$  

最后用 FC 处理 \\(s_{f}\\) 得到输入的图像与句子之间的最终相似度。  

## 目标函数与推断策略

### 目标函数

三元组损失得到 \\(\mathcal{L}\_{r}\\) 和 \\(\mathcal{L}\_{f}\\)。  

### 联合训练 (joint training)

结合 \\(\mathcal{L}\_{r}\\) 和 \\(\mathcal{L}\_{f}\\) 同时训练 SGR 和 SAF，共享相似度表征。  

### 独立训练 (independent training)

分别训练 SGR 和 SAF，推断时用两个模块输出相似度的均值。  

![Tab 5](/images/2021/PRN31/5.png)

## 模块可视化

![Fig 3](/images/2021/PRN31/3.png)
