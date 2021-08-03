---
title: "[论文阅读笔记 -- ViT] Transformer in Convolutional Neural Networks (2021)"
date: 2021-08-03T12:09:03+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "ViT", "CNN"]
draft: false
---

# 2106.03180 Transformer in Convolutional Neural Networks (2021)

[开源代码传送门](https://github.com/yun-liu/TransCNN)

## 概览

本文提出层级化的多头注意力 (H-MHSA)。  

为了结合 CNN 与 Transformer 的优势，提出 Transformer in Convolutional Neural Networks (TransCNN) 的概念，TransCNN 直接处理 3D 特征图，而非序列化数据。  

![Fig 1](/images/2021/PRN70/1.png)

![Tab 1](/images/2021/PRN70/T1.png)

![Tab 2](/images/2021/PRN70/T2.png)
