---
title: "[论文阅读笔记 -- 跨模态检索] Probabilistic Embeddings for Cross-Modal Retrieval (CVPR 2021)"
date: 2021-08-19T11:43:18+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "probability"]
draft: false
---

# Probabilistic Embeddings for Cross-Modal Retrieval (CVPR 2021)

[开源代码传送门](https://github.com/naver-ai/pcme)

![Fig 1](/images/2021/PRN81/1.png)

## 背景

一张图像可能与多条不同的文本相匹配，一条文本也可能对应多张不同的图像。  

本文提出一种概率跨模态嵌入模型 (Probabilistic Cross-Modal Embedding, PCME)。  

![Fig 2](/images/2021/PRN81/2.png)

## 联合视觉-文本嵌入 (Joint Visual-Textual Embeddings)

### 视觉编码器

ResNet  

### 文本编码器

GloVe + bi-GRU  

### 以往工作使用的损失

通常使用对比损失或三元组损失学习联合嵌入。  

### 多义视觉-语义嵌入 (Polysemous Visual-Semantic Embeddings, PVSE)

用于建模跨模态检索中的一对多匹配。  

在特征提取后采用多头注意力块，为各模态编码 \\(K\\) 个可能的嵌入，使用多实例学习损失 (multiple instance learning, MIL)，只有最好的特征对被用于监督学习。  

## 面向单个模态的概率嵌入

PCME 将各样本建模为一个分布，基于 Hedged Instance Embeddings (HIB) 构建， HIB 训练一个概率映射 \\(p\_{\theta}(z|x)\\)，其不仅保留一对样本的语义相似性，且表征数据中的内在不确定性 (inherent uncertainty)。  

其关键部件如下：  

### Soft Constrastive Loss

对于一对样本 \\((x_{\alpha}, x_{\beta})\\)，该损失定义为  

$$\mathcal{L}\_{\alpha \beta}(\theta) = \begin{cases} -log p\_{\theta}(m|x_{\alpha}, x_{\beta}) \quad if \ \alpha, \beta \ is \ a \ match \\\\ -log (1 - p_{\theta}(m|x_{\alpha}, x_{\beta})) \quad otherwise \end{cases},$$

其中 \\(p_{\theta}(m|x_{\alpha}, x_{\beta})\\) 为匹配似然 (match probability)。  

### 分解匹配似然

通过蒙特-卡洛方法分解匹配似然：  

$$p_{\theta}(m|x_{\alpha}, x_{\beta} \approx \frac{1}{J^2} \sum_{j}^{J} \sum_{j'}^{J} p(m|z_{\alpha}^{j}, z_{\beta}^{j'}),$$

其中 \\(z^{j}\\) 是采样自嵌入分布 \\(p_{\theta}(z|x)\\) 的样本。为了能够训练，嵌入分布应当对再参数化方法友好 (reparametrization-trick-friendly)。  

### 基于欧几里德距离的匹配似然

$$p(m|z_{\alpha}, z_{\beta}) = \sigma(-a||z_{\alpha} - z_{\beta}||\_{2} + b),$$  

其中 \\((a, b)\\) 是可学习的参数。  

## 概率跨模态嵌入 (PCME)

$$p(v|i) \sim N(h_{\mathcal{V}}^{\mu}(z_{v}), diag(h_{\mathcal{V}}^{\sigma}(z_{v}))),$$
$$p(t|c) \sim N(h_{\mathcal{T}}^{\mu}(z_{t}), diag(h_{\mathcal{T}}^{\sigma}(z_{t}))).$$

![Fig 3](/images/2021/PRN81/3.png)

### 局部注意力分支

受 PVSE 启发，在计算均值方差时引入局部注意力分支。  

### 损失函数

soft cross-modal contrastive loss + KL 散度 + uniformity loss  

### 衡量面向实例的不确定性

计算协方差矩阵的行列式或 \\(\sigma's\\) 的几何平均数得到。  
