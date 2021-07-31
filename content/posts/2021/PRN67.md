---
title: "[论文阅读笔记 -- 频域] Wavelet Pooling for Convolutional Neural Networks (ICLR 2018)"
date: 2021-07-31T19:43:38+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# Wavelet Pooling for Convolutional Neural Networks (ICLR 2018)

![Fig 1](/images/2021/PRN67/1.png)
![Fig 2](/images/2021/PRN67/2.png)
![Fig 3](/images/2021/PRN67/3.png)

## 概述

本文提出一种小波池化算法，使用二阶小波分解对特征进行子采样，放弃了最近临插值方法，而采用了一种子带方法，从而得以使用更少人造特征更加精确地表征特征内容。  
## 小波池化

### 前向传播

采用快速小波变换 (FWT) 在小波域中对特征图执行二阶分解，丢弃部分子带后，用 IFWT 重构。  

![Fig 4](/images/2021/PRN67/4.png)

### 反向传播

![Fig 5](/images/2021/PRN67/5.png)
