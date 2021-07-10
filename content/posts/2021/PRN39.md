---
title: "[论文阅读笔记 -- 频域 / 图像恢复] Multi-level Wavelet-CNN for Image Restoration (CVPR 2018)"
date: 2021-07-10T16:41:48+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "wavelet", "frequency"]
draft: false
---

# Multi-level Wavelet-CNN for Image Restoration (CVPR 2018)

![Fig 1](/images/2021/PRN39/1.png)

## 背景

### 图像恢复 (Image Restoration) 的目的

从观测到的较差图像 \\(y\\) 恢复潜在的干净图像 \\(x\\)。  

本文设计了一种多级小波 CNN (multi-level wavelet CNN, MWCNN) 模型，增大感受野，改善表现与效率之间的权衡。  

![Fig 2](/images/2021/PRN39/2.png)

## 从多级 WPT 到 MWCNN

2D 离散小波变换 (DWT) 使用 4 种滤波器与图像 \\(x\\) 进行卷积，卷积结果下采样后得到四个子带图 (subband images)，\\(x\\) 可由逆小波变换 (IWT) 重构得到：\\(x = IWT(x_{1}, x_{2}, x_{3}, x_{4})\\)。  

在多级小波包变换 (multi-level wavelet packet transform, WPT) 中，子带图进一步用 DWT 处理生成分解结果，在二级 WPT 中，各子带图被分解为四个子带图，以此类推。实际上 WPT 是 FCN 不带非线性层的一种特殊情况。  

本文进一步拓展 WPT，在两级 DWT 之间加入 CNN 块，构建多级小波 CNN (MWCNN)。  

## 网络架构

![Fig 3](/images/2021/PRN39/3.png)

MWCNN 架构的核心在于各级 DWT 后的 CNN 块，实现为 4 层不带池化的 FCN，每一层为 \\(3 \times 3\\) 卷积 + BN + ReLU，最后一个 CNN 块的最后一层不带 BN 和 ReLU。  

默认使用 Haar 小波，实验中也考虑了 Daubechies 2 (DB2) 等其他小波。  

![Fig 4](/images/2021/PRN39/4.png)
