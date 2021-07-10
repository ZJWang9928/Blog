---
title: "[论文阅读笔记 -- 多模态预训练] Unicoder-VL (AAAI 2020)"
date: 2021-07-10T10:00:50+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "transformer", "ViT", "pre-training"]
draft: false
---

# Unicoder-VL: A Universal Encoder for Vision and Language by Cross-Modal Pre-Training (AAAI 2020)

![Fig 1](/images/2021/PRN36/1.png)

## 背景

尚未出现能够够直接处理跨模态任务与数据的预训练模型。  

本文基于多层 Transformer 提出一种通用视觉语言编码器 (Universal Encoder for Vision And Language, Unicoder-VL)。  

### 用到的三种预训练任务

+ Masked Language Modeling (MLM)
+ Masked Object Classification (MOC)
+ Visual-linguistic Matching (VLM)
