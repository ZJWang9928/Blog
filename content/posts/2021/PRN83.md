---
title: "[论文阅读笔记 -- 跨模态检索] HAL: Mitigating Visual Semantic Hubs (AAAI 2020)"
date: 2021-08-26T12:55:43+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "hubness"]
draft: false
---

# HAL: Improved Text-Image Matching by Mitigating Visual Semantic Hubs (AAAI 2020)

![Fig 1](/images/2021/PRN83/1.png)

## 背景

本文主要针对图文匹配任务中的[枢纽点问题 (Hubness Problem)](http://jonathanwayy.xyz/2021/ldp_hubness_problem/) 进行研究。  

由于图文匹配中的嵌入空间是由联合建模视觉和语言得到的，通常将其成为视觉语义嵌入 (Visual Semantic Embeddings, VSE)。  

本文提出一种自适应的枢纽点感知损失 (self-adjustable hubness-aware loss)，称为 HAL，其既考虑从整个训练集采样而来的全局统计量，也考察从 mini-batch 得到的局部统计量，利用枢纽的信息自动调整采样权重。其既从困难样本进行学习，也由于考虑了多个样本而对噪声健壮。  

为了决定权重，对一个样本考察如下两种关系：  
+ 其与 mini-batch 内样本的关系
+ 其与一个 memory bank 中 k 近邻 query 的关系

## 两种基于三元组的损失函数

### Sum-margin Loss (SUM)

最大的不足在于平等看待 mini-batch 中所有的三元组，为之赋予相同的权重。  

### Max-margin Loss (MAX)

其梯度容易收到噪声的主导，训练数据中的伪最困难负样本 (pseudo hardest negatives) 是噪声的一个主要来源。  

二者均同时之考察一个匹配对和一个适配对。  

## The Hubness-Aware Loss (HAL)
