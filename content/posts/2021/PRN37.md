---
title: "[论文阅读笔记 -- ViT / 跨模态检索] Fine-grained Visual Textual Alignment (2020)"
date: 2021-07-10T11:14:29+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "cross-modal", "retrieval"]
draft: false
---

# 2008.05231 Fine-grained Visual Textual Alignment for Cross-Modal Retrieval using Transformer Encoders (2020)

## 背景

在分类任务上预训练的 CNN 网络所提取的特征通常只能捕捉到图像的全局描述，而忽视了重要的局部细节。  

### 现有方法的问题

由于交叉注意力或记忆力层的存在，难以分开提取图文特征，效率低下。  

本文提出 Transformer Encoder Reasoning and Alignment Network (TERAN)，进行细粒度的单词-区域对齐。  

## Transformer 编码器回顾

![Fig 1](/images/2021/PRN37/1.png)

## TERAN

![Fig 2](/images/2021/PRN37/2.png)

### 特征提取

Faster-RCNN 提取视觉特征，BERT 提取文本特征。  

### 目标函数

hinge-based triplet ranking loss  

### 区域-单词对齐矩阵

用最后一个 TE 层的输出特征计算得到区域-单词对齐矩阵 \\(A \in \mathbb{R}^{|g_{k}| \times |g_{l}|}\\)，用余弦相似度衡量区域与单词之间的关联度，为了最终得到全局的相似度，采用了最大-求和池化 (max-sum pooling)，具体分为 max-over-regions sum-over-words (\\(M_{r}S_{w}\\)) pooling 和 max-over-words sum-over-regions (\\(M_{w}S_{r}\\)) pooling，二者得到的注意力最终求和。  

## Normalized Discouted Cumulative Gain (NDCG)

用于通过查看排序列表前 \\(p\\) 项衡量根据某个 query 排序的质量。  
