---
title: "[论文阅读笔记 -- ViT / 跨模态检索] Revamping Cross-Modal Recipe Retrieval (CVPR 2021)"
date: 2021-07-28T10:41:00+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "cross-modal", "retrieval"]
draft: false
---

# Revamping Cross-Modal Recipe Retrieval with Hierarchical Transformers and Self-supervised Learning (CVPR 2021)

[开源代码传送门](https://github.com/amzn/image-to-recipe-transformers)

![Fig 1](/images/2021/PRN61/1.png)

## 图像编码器 \\(\phi_{img}\\)

ResNet-50 / ResNeXt / ViT  

## 菜谱编码器 \\(\phi_{rec}\\)

### 三类数据要处理
+ 标题
+ 成分
+ 指导

用三个独立的基于 Transformer 的编码器分别处理三种数据。  

### 句子表征 (TR)

两层 Transformer 网络，\\(D = 512\\)，4 个 head，第一层加入位置编码，最后一层输出特征的均值作为句子表征。  

### 层叠式表征 (HTR)

首先一组 Transformer 提取各句子特征，进而用句子特征作为输入用 Transformer 汇聚成一个特征，不同成分拼接后最终再做一次变换。  

![Fig 2](/images/2021/PRN61/2.png)

## 自监督菜谱损失 \\(\mathcal{L}\_{rec}\\)

有时数据并不能成对获取，比如只有菜谱没有图片。  

在菜谱各类数据之间引入三元组损失。  

计算损失前再加一步映射，将一个特征映射到另一个特征的空间里。  

![Fig 3](/images/2021/PRN61/3.png)
