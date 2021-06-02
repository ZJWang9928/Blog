---
title: "[论文阅读笔记 -- VQA] Reasoning with Heterogeneous Graph Alignment (AAAI 2020)"
date: 2021-06-02T18:30:46+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
<!-- draft: false -->
draft: true
---

# Reasoning with Heterogeneous Graph Alignment for Video Question Answering

![Fig 1](/images/2021/PRN10/1.png)

## 出发点

需要一个统一的方法同步对模态间和模态内的关联性进行建模与推理。  

![Fig 2](/images/2021/PRN10/2.png)

本文所提到的 “视频段 (video shot)” 指的是一小段能被 3D 卷积模块处理并产生一个动作向量 (motion vector) 的视频片段。  

## 视频与文本上下文特征

### 视觉模态

由于视频段比帧级别具有更丰富的表达能力，因此使用 3D 卷积网络 C3D 处理得到视频段级别的视频动作特征；同时使用 2D 卷积网络 ResNet 提取帧级别特征作为补充。从而得到外观特征组 \\(F_{A}\\) 和动作特征组 \\(F_{M}\\)，各包含 \\(L_{v}\\) 个特征， \\(L_{v}\\) 是视频的帧数，拼接后用两个 FC 层将其投影到一个公共视觉空间之中，得到特征组 \\(F\\)，也包含 \\(L_{v}\\) 个特征。  

### 文本模态

GloVe 进行词嵌入得到 \\(E\\)，句子长度为 \\(L_{q}\\)。  

### 聚合动态时域信息得到上下文特征

用两个独立的 GRU 分别处理视觉与文本模态的特征组 \\(F\\) 和 \\(E\\)，得到最终的视频特征 \\(V \in \mathbb{R}^{L_{v} \times d}\\) 和问题特征 \\(Q \in \mathbb{R}^{L_{q} \times d}\\)。  

## 跨模态联合融合与对齐 (Cross-Modal Joint Fusion and Alignment)

## 异构图推理 (Heterogeneous Graph Reasoning)

## 全局与局部信息融合

## 答案预测与评估

![Fig 3](/images/2021/PRN10/3.png)
