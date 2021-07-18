---
title: "[论文阅读笔记 -- 跨模态检索] Heterogeneous Attention Network (SIGIR 2021)"
date: 2021-07-18T19:39:50+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "cross-modal", "retrieval", "transformer", "ViT"]
draft: false
---

# Heterogeneous Attention Network for Effective and Efficient Cross-modal Retrieval (SIGIR 2021)

![Fig 1](/images/2021/PRN54/1.png)

## 背景

### 联合嵌入 (joint embedding) 方法的问题
+ 只进行全局匹配
+ 文本与图像只在匹配阶段有交互

多模态的 Transformer 系方法能够在较早的阶段实现跨模态交互，但是计算量巨大。  

本文提出一种异质注意力网络 (Heterogeneous Attention Network, HAN)。  

### 组成 HAN 的四个模块
+ 图文编码模块
+ 异质注意力模块
+ 匹配模块
+ 损失模块

## 特征编码

### 视觉特征

Faster-RCNN + FC

### 文本特征

Bi-GRU

## 异质注意力模块

模块的输入由用 \\(\mathcal{l}\_{2}\\) 范数规范化后的单词特征与选框特征拼接得到：  

$$[h_{1}, \cdots, h_{m+n}] = [\frac{o_{1}}{||o_{1}||\_{2}}, \cdots, \frac{o_{m}}{||o_{m}||\_{2}}, \frac{w_{1}}{||w_{1}||\_{2}}, \cdots, \frac{w_{n}}{||w_{n}||\_{2}}].$$  

异质注意力模块由若干自注意力块堆叠而成：  

$$q_{i} = W_{q}h_{i} + b_{q}, \ k_{i} = W_{k}h_{i} + b_{k}, \ v_{i} = W_{v}h_{i} + b_{v},$$
$$a_{i, j} = q_{i}^Tk_{j}, \ i,j \in [1, \cdots, m+n],$$
$$\hat{a}\_{i, j} = \frac{e^{\beta a_{i,j}}}{\sum_{k=1}^{n+m} e^{\beta a_{i, k}}}, \ i,j \in [1, \cdots, m+n],$$
$$\hat{h}\_{i} = [v_{1}, v_{2}, \cdots, v_{n+m}]a_{i}, \ i \in [1, \cdots, m+n],$$
$$\widetilde{h}\_{i} = fc(\hat{h}\_{i}) + q_{i}, \ i \in [1, \cdots, m+n],$$

## 对齐与匹配模块

得到跨模态注意力处理后的图文特征：  

$$\hat{w}\_{i} = \widetilde{h}\_{i}, \ i \in [1, \cdots, n],$$
$$\hat{o}\_{i} = \widetilde{h}\_{n + i}, \ i \in [1, \cdots, m],$$

### 对齐

两两计算相似度并求 softmax，对视觉特征做加权和得到各文本特征 \\(\hat{w}\_{i}\\) 对应的 \\(\widetilde{o}\_{i}\\)。  

### 匹配

计算 \\(\hat{w}\_{i}\\) 与 \\(\widetilde{o}\_{i}\\) 之间余弦相似度得到 \\(r_{i}\\)，求平均得到 \\(S_{AVG}\\)。  

## 算法描述

![Alg 1](/images/2021/PRN54/A1.png)

## 损失模块

提出一种 soft-max 三元组损失：  

$$\mathcal{L}\_{soft} = \sum_{i=1}^K \\{[\sum_{j \ne i}[S_{ij} - S{ii} + \mu]\_{+}^p]^{1/p} + [\sum_{i \ne j}[S_{ij} - S{jj} + \mu]\_{+}^p]^{1/p}\\},$$  

其中 \\(p > 1\\)，上式中可以如下改写：  

$$[S_{ij} - S{ii} + \mu]\_{+}^p = w_{ij}[S_{ij} - S{ii} + \mu]\_{+}^{p-1},$$

其中

$$w_{ij} = [S_{ij} - S{ii} + \mu]\_{+}^{p-1}.$$

由于 \\(p > 1\\),\\([S_{ij} - S{ii} + \mu]\\) 越大则 \\(w_{ij}\\) 越大，从而给了更困难的样本更高的权重，且同时考虑了所有负样本，\\(p = 1\\) 时退化为全部直接求和的形式，而 \\(p \rightarrow +\infty\\) 时退化为最负样本的形式。  
