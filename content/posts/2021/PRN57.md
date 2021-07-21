---
title: "[论文阅读笔记 -- ViT / ReID] TransReID: Transformer-based Object Re-Identification (2021)"
date: 2021-07-21T10:10:54+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "retrieval", "reid"]
draft: false
---

# 2102.04378v2 TransReID: Transformer-based Object Re-Identification (2021)

[开源代码传送门](https://github.com/heshuting555/TransReID)

![Fig 1](/images/2021/PRN57/1.png)

## 背景

### 目标 ReID 尚未很好解决的两个问题
+ 从全局视角提取丰富的结构模式
+ 包含细节信息的细粒度特征提取受到下采样的限制

![Fig 2](/images/2021/PRN57/2.png)

本文提出一种 TransReID 架构，包含 jigsaw patches module (JPM) 与 side information embedding (SIE) 两个主要部件。  

![Fig 4](/images/2021/PRN57/4.png)

## Baseline 模型架构

![Fig 3](/images/2021/PRN57/3.png)

### 重叠的 Patch

不同与 ViT、DeiT 等方法，本文用滑窗生成有像素重叠的 patch。

步长为 \\(S\\)，patch 边长为 \\(P\\)，两个邻接 patch 的重叠面积为 \\((P-S) \times P\\)。  

## Jigsaw Patch Module (JPM)

Baseline 用到了整张图像的信息，但是由于遮挡与不对齐问题，对一个目标的观测可能并不完整，CNN 系列的方法利用如条带特征等细粒度信息应对这一问题。  

JPM 打乱 patch embedding 并将其重新分组，每组提取一个局部特征。此外还有一个与之并行的全局分支。  

### 具体步骤
+ Shift 操作：除了 [cls] 之外的前 \\(m\\) 个 patch 移动至最后
+ Patch Shuffle 操作：打乱分为 \\(k\\) 组

## Side Information Embeddings (SIE)

用于结合非视觉信息，如相机、视角等。  

实现为可学习的 1-D 嵌入。  
