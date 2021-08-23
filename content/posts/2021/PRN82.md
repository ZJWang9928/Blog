---
title: "[论文阅读笔记 -- 跨模态检索] Step-Wise Hierarchical Alignment Network (IJCAI 2021)"
date: 2021-08-23T14:43:44+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "probability"]
draft: false
---

# Step-Wise Hierarchical Alignment Network for Image-Text Matching (IJCAI 2021)

![Fig 1](/images/2021/PRN82/1.png)

## 背景

### 现有方法的问题
根据明显差异来鉴别图文对，可能会因而无法区分具有微小上下文信息差异但是语义内容相似的样本。  

本文提出一种逐步层级化对齐网络 (Step-wise Hierarchical Alignment Network, SHAN)，分两个语义级别进行层级化的跨模态对齐：  
+ fragment-level alignment (local-to-local)
+ context-level alignment (global-to-local + global-to-global)

![Fig 2](/images/2021/PRN82/2.png)

## 特征提取

### 图像表征

Faster R-CNN  

提取 36 个局部区域的特征。  

### 文本表征

Glove 与一个可学习的嵌入层分别嵌入得到一个特征，拼接二者作为单词的特征，用 bi-GRU 进一步处理，双向均值作为输出。  

## Step-wise Hierarchical Alignment

### Fragment-level L2L Alignment

首先计算区域-单词相关性矩阵：  

$$A = (\widetilde{W}\_{v}V)(\widetilde{W}\_{t}T)^T \ \in \mathbb{R}^{k \times n},$$

\\(A_{ij}\\) 表示区域 \\(i\\) 与单词 \\(j\\) 之间的语义相似度。  

分别计算得到 attended text-level representation \\(t^{\*}\_{i}\\) 与 attended image-level representation \\(v^{\*}\_{j}\\)：  

$$t^{\*}\_{i} = \sum\_{j=1}^{n} \alpha_{ij} t_{j},$$
$$\alpha_{ij} = \frac{exp(\lambda A_{ij})}{exp(\sum_{j=1}^n \lambda A_{ij})},$$
$$v^{\*}\_{j} = \sum\_{i=1}^{k} \beta_{ij} v_{i},$$
$$\beta_{ij} = \frac{exp(\lambda A_{ij})}{exp(\sum_{i=1}^k \lambda A_{ij})}.$$  

区域相关的与单词相关的 L2L 匹配分数分别定义为  

$$R_{v}(v_{i}, t^{\*}\_{i}) = \frac{v_{i} t^{\*T}\_{i}}{||v_{i}|| ||t_{i}^{\*}||}, \ i \in \{1, 2, \cdots, k\},$$
$$R_{t}(t_{j}, v^{\*}\_{j}) = \frac{t_{j} v^{\*T}\_{j}}{||t_{j}|| ||v_{j}^{\*}||}, \ j \in \{1, 2, \cdots, n\},$$

对二者的总和进行加权求和。  

### Context-level G2L Alignment

通过融合与池化操作得到全局表征，首先进行面向元素的融合操作：  

$$g = sigmoid(W_{g} cat(v_{i}, t_{i}^{\*}) + b_{g}),$$
$$v_{i}^c = g \* v_{i} + (1 - g) \* t_{i}^{\*}.$$  

文本模态处理与之类似。  

进而用自注意力机制得到权重以聚合得到的特征。  

G2L 匹配较为常规。  

### Context-level G2G Alignment

对位相加得到最终的全局表征：  

$$v_{g} = v_{c} \oplus t_{c}^{\*},$$
$$t_{g} = t_{c} \oplus v_{c}^{\*}.$$

计算二者之间的余弦相似度。  

## 目标函数

三元组损失。  

## 注意力权重可视化

![Fig 3](/images/2021/PRN82/3.png)
