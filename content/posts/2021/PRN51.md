---
title: "[论文阅读笔记 -- 频域 / ViT] M2TR: Multi-modal Multi-scale Transformers (2021)"
date: 2021-07-17T14:34:02+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "transformer", "attention", "ViT"]
draft: false
---

# 2104.09770 M2TR: Multi-modal Multi-scale Transformers for Deepfake Detection (2021)

![Fig 1](/images/2021/PRN51/2.png)

## 背景

Deepfake 检测任务。  

本文提出一种多模态多尺度 Transformer (Multi-modal Multi-scale Transformer, M2TR)，包含一个多尺度 Transformer 模块 (MT) 和一个跨模态融合模块 (CMF)，利用频域信息捕捉更多细节。  

## 多尺度 Transformer

![Fig 2](/images/2021/PRN51/3.png)

## 跨模态融合 (CMF)

首先用 DCT 将输入的图像 \\(X\\) 从 RGB 域变换到频域，得到 \\(\mathcal{DCT}(X) \in \mathbb{R}^{H \times W \times 1}\\)。  

由于 DCT 的性质，低频响应位于 \\(\mathcal{DCT}(X)\\) 的左上角，高频响应位于右下角。  

利用三种人工设计的二值基滤波器 \\(\\{t_{base}^{i}\\}\_{i=1}^{3}\\) 将频域分为低、中、高三种频带，得到分解后的频率成分：  

$$d_{i} = \mathcal{DCT}(X) \odot t_{base}^{i}, i = \\{1, 2, 3\\}.$$  

低频带为整个谱的前 \\(1/16\\)，中频带为 \\(1/16\\) 到 \\(1/8\\)，高频带为后 \\(7/8\\)。  

接着用 IDCT 将 \\(d_{i}\\) 变换会 RGB 域得到 \\(b_{i} = \mathcal{DCT}^{-1}(d_{i})\\)，最终沿着通道维拼接得到 \\(B \in \mathbb{H \times W \times 3}\\)，输入几个卷积层提取频率特征 \\(f_{fq}\\)。  

CMF 用于融合 RBG 特征 \\(f_{s}\\) 与频率特征 \\(f_{fq}\\)。使用 query-key-value 机制，用 \\(1 \times 1\\) 卷积将两种特征嵌入  

$$Q = conv_{q}(f_{s}), \ K = Conv_{k}(f_{fq}), \ V = Conv_{v}(f_{fq}).$$  

沿着空间维度拉平得到 2D 嵌入 \\(\widetilde{Q}, \widetilde{K}, \widetilde{V} \in \mathbb{R}^{(HW/16) \times C}\\)，计算融合特征  

$$f_{fuse} = softmax(\frac{\widetilde{Q}\widetilde{K}^T}{\sqrt{H/4 \times W/4 \times C}})\widetilde{V}.$$  
