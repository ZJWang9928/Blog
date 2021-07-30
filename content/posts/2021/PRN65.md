---
title: "[论文阅读笔记 -- 频域] Wavelet Integrated CNNs for Noise-Robust Img Classification(CVPR 2020)"
date: 2021-07-30T19:49:00+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# Wavelet Integrated CNNs for Noise-Robust Image Classification (CVPR 2020)

[开源代码传送门](https://github.com/LiQiufu/WaveCNet)

![Fig 1](/images/2021/PRN65/1.png)

## 概述

CNN 对噪声的鲁棒性较差，随机噪声大多为高频成分，CNN 在下采样之前缺少滤波步骤，可能会导致高低频成分的混叠。  

本文通过将小波与 CNN 结合来抑制噪声的影响。将 DWT 与 IDWT 构建为 PyTorch 中的层，用 DWT 取代下采样后设计了结合小波的卷积网络 (wavelet integrated convolutional network, WaveCNet)，旨在通过小波变换加强深度网络中的下采样操作。  

在下采样过程中，WaveCNet 消除特征图中的高频成分，从低频成分中提取高级信息。  

## DWT 与 IDWT 层

对于 2D 信号 \\(x\\)，DWT 分别沿着行和列作 1D DWT：  

$$X_{ll} = LXL^T,$$
$$X_{lh} = HXL^T,$$
$$X_{hl} = LXH^T,$$
$$X_{hh} = HXH^T.$$

相应的 IDWT 实现为：  

$$X = L^TX_{ll}L + H^TX_{lh}L + L^TX_{hl}H + H^TX_{hh}H.$$  

对于多通道数据，逐层进行 DWT 与 IDWT。  

![Fig 2](/images/2021/PRN65/2.png)

## WaveCNets

### 通常基于小波的降噪方法分为三步
+ 用 DWT 将 \\(X\\) 分解为低频与高频成分
+ 对高频成分进行滤波
+ 用 IDWT 重构数据

### 本文方法
本文采用最简单的基于小波的降噪方法，即直接丢弃高频成分，这样的 \\(DWT_{ll}\\) 运算后将特征图长宽减半，与 CNN 中部件的替换关系如下：  

![Fig 3](/images/2021/PRN65/3.png)

## 特征图示例

![Fig 5](/images/2021/PRN65/5.png)

![Fig 7](/images/2021/PRN65/7.png)
