---
title: "[论文阅读笔记 -- 跨模态检索] Context-Aware Attention Network (CVPR 2020)"
date: 2021-08-04T19:43:07+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention"]
draft: false
---

# Context-Aware Attention Network for Image-Text Retrieval (CVPR 2020)

![Fig 1](/images/2021/PRN71/1.png)

## 背景

### 现有方法的问题
+ 不同的局部块有不同重要性
+ 忽略同模态中各局部之间的语义相关性
+ 一个单词或一个图像区域在不同的全局上下文中可能有不同的语义

本文提出一种知道上下文的注意力网络 (Context-Aware Attention Network, CAAN)，基于全局上下文选择局部部件。  

![Fig 2](/images/2021/PRN71/2.png)

## 特征提取

### 视觉表征

Faster R-CNN

### 文本表征

Bi-GRU  

## Context-Aware Attention

### 公式

视觉特征 \\(V\\) 与文本特征 \\(U\\) 之间计算相似度矩阵：  

$$H = tanh(V^TKU),$$  

区域的注意力函数为：  

$$\widetilde{f}(V, U) = tanh(H^vV^TQ_{1} + H^{uv}U^TQ_{2}),$$
$$f(V, U) = softmax(W^v \widetilde{f}(V, U)).$$  

单词的注意力函数为：  

$$\widetilde{g}(V, U) = tanh(H^uU^TQ_{3} + H^{vu}V^TQ_{4}),$$
$$g(V, U) = softmax(W^u \widetilde{g}(V, U)).$$  

而后有  

$$\hat{v} = Vf(V, U),$$
$$\hat{u} = Ug(V, U).$$  

### 模态间注意力 \\(H^{uv}, H^{vu}\\)

word-to-region attention:  

$$H^{uv}\_{ij} = \frac{[H_{ij}]\_{+}}{\sqrt{\sum\_{k=1}^n [H_{kj}]\_{+}^2}},$$

region-to-word attention:  

$$H^{vu}\_{ij} = \frac{[H_{ij}]\_{+}}{\sqrt{\sum\_{k=1}^m [H_{ik}]\_{+}^2}},$$

### 模态内注意力 \\(H^{v}, H^{u}\\)

本文讨论了两种版本的模态内注意力。  

#### 基于特征的注意力 (FA)

$$H^v = V^TM_{1}V,$$
$$H^u = U^TM_{2}U.$$

#### 基于语义的注意力 (SA)

$$H^u = \begin{bmatrix} norm(H^{vu}\_{1}) \\\\ norm(H^{vu}\_{2}) \\\\ ... \\\\ norm(H^{uv}\_{n}) \end{bmatrix} \begin{bmatrix} norm(H^{uv}\_{1}) \\\\ norm(H^{uv}\_{2}) \\\\ ... \\\\ norm(H^{uv}\_{n}) \end{bmatrix}^T,$$
$$H^v = \begin{bmatrix} norm(H^{uv}\_{1}) \\\\ norm(H^{vu}\_{2}) \\\\ ... \\\\ norm(H^{vu}\_{m}) \end{bmatrix} \begin{bmatrix} norm(H^{vu}\_{1}) \\\\ norm(H^{vu}\_{2}) \\\\ ... \\\\ norm(H^{vu}\_{m}) \end{bmatrix}^T,$$

![Fig 3](/images/2021/PRN71/3.png)

## 可视化

![Fig 4](/images/2021/PRN71/4.png)
