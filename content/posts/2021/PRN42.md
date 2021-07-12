---
title: "[论文阅读笔记 -- 跨模态检索] CAMP: Cross-Modal Adaptive Message Passing (ICCV 2019)"
date: 2021-07-12T10:58:13+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "vqa"]
draft: false
---

# CAMP: Cross-Modal Adaptive Message Passing for Text-Image Retrieval (ICCV 2019)

[开源代码传送门](https://github.com/ZihaoWang-CV/CAMP_iccv19)

![Fig 1](/images/2021/PRN42/1.png)

## 背景

### 现有方法的问题

现有方法通常学习一个公共的嵌入空间，在其中衡量特征相似度，使用 ranking loss 进行训练。  

这类方法没有充分挖掘图文数据之间的信息交互，所提取的可能并不是最优特征。  

### 一个特殊性

不同于如 VQA 等问题，信息融合时的多模态特征并不总是匹配的。  

### 本文的解决方案

本文提出一种跨模态自适应消息传递模型 (Cross-modal Adaptive Message Passing model, CAMP)，由跨模态消息聚合模块 (Cross-modal Message Aggregation module) 以及跨模态门控融合模块 (Cross-modal Gated Fusion module) 构成。  

自适应地控制来自另一模态的消息应当以何种程度与原始特征融合。  

直接更具融合后的特征预测跨模态匹配分数，用交叉熵训练；而非特征距离 + Ranking Loss。  

![Fig 2](/images/2021/PRN42/2.png)

## 跨模态消息聚合

基于跨模态注意力机制。  

$$A = (\widetilde{W}\_{v}V)^T(\widetilde{W}\_{t}T) \in \mathbb{R}^{R \times N},$$
$$\widetilde{A}\_{v} = softmax(\frac{A^T}{\sqrt{d_{h}}}),$$
$$\widetilde{V} = \widetilde{A}\_{v}V^T \in \mathbb{R}^{N \times d},$$
$$\widetilde{A}\_{t} = softmax(\frac{A}{\sqrt{d_{h}}}),$$
$$\widetilde{T} = \widetilde{A}\_{t}T^T \in \mathbb{R}^{N \times d}.$$

![Fig 3](/images/2021/PRN42/3.png)

## 跨模态门控融合

传统的融合操作假设图文特征是匹配的，这与检索任务的特性不符。  

### \\(V\\) 与 \\(\widetilde{T}\\) 融合

$$g_{i} = \sigma(v_{i} \odot \widetilde{t}\_{i}^T), \quad i \in \\{1, \cdots, R\\},$$
$$G_{v} = [g_{1}, \cdots, g_{R}] \in \mathbb{R}^{d \times R}.$$
$$\hat{V} = \mathcal{F}\_{v}(G_{v} \odot (V \oplus \widetilde{T}^T)) + V.$$

### \\(T\\) 与 \\(\widetilde{V}\\) 融合

$$h_{i} = \sigma(\widetilde{v}\_{i}^T \odot t_{i}), \quad i \in \\{1, \cdots, N\\},$$
$$H_{t} = [h_{1}, \cdots, h_{R}] \in \mathbb{R}^{d \times N}.$$
$$\hat{T} = \mathcal{F}\_{t}(H_{t} \odot (T \oplus \widetilde{V}^T)) + T.$$

## 融合后特征的聚合

$$a_{v} = softmax(\frac{W_{v}\hat{V}}{\sqrt{d}})^T \in \mathbb{R}^{R},$$
$$v^{\*} = \hat{V}a_{v} \in \mathbb{R}^d.$$
$$a_{t} = softmax(\frac{W_{t}\hat{T}}{\sqrt{d}})^T \in \mathbb{R}^{N},$$
$$t^{\*} = \hat{T}a_{t} \in \mathbb{R}^d.$$

## 推断

将图文匹配变成分类问题，即匹配与失配两类，用两层 MLP 与 sigmoid 激活函数计算最终的匹配度分数：  

$$m(\mathcal{I}, \mathcal{T}) = \sigma(MLP(v^{\*} + t^{\*})).$$

用  hardest negative cross-entropy loss 训练模型：  

$$\mathcal{L}\_{BCE-h}(\mathcal{I}, \mathcal{T}) = log(m(\mathcal{I}, \mathcal{T})) + max_{\mathcal{T}'}[log(1 - m(\mathcal{I}, \mathcal{T}'))]$$
$$+ log(m(\mathcal{I}, \mathcal{T})) + max_{\mathcal{I}'}[log(1 - m(\mathcal{I}', \mathcal{T}))].$$

## 检索结果示例

![Fig 4](/images/2021/PRN42/4.png)
