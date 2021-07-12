---
title: "[论文阅读笔记 -- 频域 / 风格迁移] Photorealistic Style Transfer via Wavelet Transforms (ICCV 2019)"
date: 2021-07-12T10:12:48+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "style transfer", "frequency", wavelet]
draft: false
---

# Photorealistic Style Transfer via Wavelet Transforms (ICCV 2019)

[开源代码传送门](https://github.com/ClovaAI/WCT2)

![Fig 1](/images/2021/PRN41/1.png)

## 背景

风格迁移任务。  

模型应当在不损伤图像细节的情况下实现风格迁移。  

本文提出一种基于白化与色彩变换的小波矫正迁移 (wavelet corrected transfer based on whitening and coloring transforms, \\(WCT^2\\))，用小波池化与反池化 (wavelet pooling and unpooling) 替换 VGG 编码器与解码器中的池化与反池化操作。  

使用 progressive 风格化而非多级策略。  

![Fig 2](/images/2021/PRN41/2.png)

## 小波矫正迁移 (Wavelet Corrected Transfer)

### Haar 小波池化与反池化

Haar 小波池化与反池化有四个核，分别是 \\(\\{LL^T, LH^T, HL^T, HH^T\\}\\)，其中低通滤波器 (\\(L\\)) 与高通滤波器 (\\(H\\)) 分别为：  

$$L^T = \frac{1}{\sqrt{2}}[1 \quad 1],$$
$$H^T = \frac{1}{\sqrt{2}}[-1 \quad 1].$$

因此不同与通常的池化操作，Haar 小波池化的输出有四个通道。  

低通滤波器波着平滑的表面与纹理，高通滤波器提取形如垂直、水平、对角线边的信息。  

小波池化的一个重要特性在于能够通过镜像化其操作重构原始信号，即小波反池化。  

![Fig S1](/images/2021/PRN41/S1.png)

### 模型架构

三种包含高频成分的通道直接跳连传输到解码器，而低频成分 \\(LL\\) 则传入下一个编码器层。  

## Progressive 风格化

![Fig S2](/images/2021/PRN41/S2.png)

## 可视化分析与结果呈现

![Fig 3](/images/2021/PRN41/3.png)

![Fig 4](/images/2021/PRN41/4.png)

![Fig 5](/images/2021/PRN41/5.png)

![Fig 6](/images/2021/PRN41/6.png)

![Fig 7](/images/2021/PRN41/7.png)

![Fig 8](/images/2021/PRN41/8.png)
