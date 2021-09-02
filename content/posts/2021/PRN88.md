---
title: "[论文阅读笔记 -- 跨模态哈希] Attention-Guided Semantic Hashing (ICME 2021)"
date: 2021-09-02T12:29:35+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "hashing", "unsupervised"]
draft: false
---

# Attention-Guided Semantic Hashing for Unsupervised Cross-Modal Retrieval (ICME 2021)

## 概述

无监督跨模态哈希问题。  

本文提出注意力指导的语义哈希模型 (Attention-Guided Semantic Hashing)。  

![Fig 1](/images/2021/PRN88/1.png)

## 模型架构

### 特征提取

VGG-16 提取图像特征，universal sentence encoder feature 作为文本特征。  

训练时固定预训练好的特征提取模块参数。  

### 注意力模块

掩码权重计算如下：  

$$M_{I} = \sigma(WX + b),$$
$$M_{T} = \sigma(WY + b),$$

二者的全连接层共享。

进而得到模块输出：  

$$G_{I} = X + M_{I} \cdot X,$$
$$G_{T} = Y + M_{T} \cdot Y.$$

### 哈希模块

用全连接处理注意力模块输出后，二值化。  

### 构建注意力可知的语义矩阵 (Attention-Aware Semantic Matrix)

对 \\(X\\) 和 \\(Y\\) 归一化后可得到特征自相似度矩阵：  

$$S_{I} = \hat{X}^T \hat{X} \in [-1, +1]^{n \times n},$$
$$S_{T} = \hat{Y}^Y \hat{X} \in [-1, +1]^{n \times n},$$

其中 \\(n\\) 是 batch size。  

再计算自注意力矩阵：  

$$A_{I} = \hat{G}^{T}\_{I} \hat{G}\_{I} \in [-1, +1]^{n \times n},$$
$$A_{T} = \hat{G}^{T}\_{T} \hat{G}\_{T} \in [-1, +1]^{n \times n}.$$

整合二者：  

$$S_{fuse} = \gamma \frac{S_{I}A_{I}^{T}}{n} + (1 - \gamma) \frac{S_{T}A_{T}^{T}}{n}.$$

进而可求得注意力可知的语义矩阵：  

$$S = \lambda S_{fuse} + (1 - \lambda) \frac{S_{fuse} S_{fuse}^{T}}{n}.$$

构建三个损失函数：  

$$L_{cross} = min_{B_{I}, B_{T}} ||S - cos(B_{I}, B_{T})||\_{F}^{2},$$
$$L_{img} = min_{B_{I}} ||S - cos(B_{I}, B_{I})||\_{F}^{2},$$
$$L_{txt} = min_{B_{T}} ||S - cos(B_{T}, B_{T})||\_{F}^{2}.$$

为了避免梯度消失问题，引入 scaled tanh 函数：  

$$B = tanh(\eta H) \in [-1, +1]^{k \times N}, \eta \in \mathbb{R}^{+}.$$
