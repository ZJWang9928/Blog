---
title: "[论文阅读笔记 -- 频域] FDA: Fourier Domain Adaptation (CVPR 2020)"
date: 2021-08-16T16:19:12+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# FDA: Fourier Domain Adaptation for Semantic Segmentation (CVPR 2020)

[开源代码传送门](https://github.com/YanchaoYang/FDA)

![Fig 1](/images/2021/PRN79/1.png)

## 概述

提出傅里叶域自适应 (FDA)。  

FFT ==> 用目标图像的低频成分替换源图像低频成分 ==> iFFT  

## FDA

设计了一个外圈置零的掩码：  

$$M_{\beta}(h, w) = \mathbb{1}\_{(h, w) \in [-\beta H: \beta H, -\beta W: \beta W]}.$$  

FDA 定义如下：  

$$x^{s \rightarrow t} = \mathcal{F}^{-1}([M\_{\beta} \circ \mathcal{F}^{A}(x^t) + (1 - M_{\beta}) \circ \mathcal{F}^{A}(x^s), \mathcal{F}^{P}(x^s)]).$$

### \\(\beta\\) 取值的影响

![Fig 2](/images/2021/PRN79/2.png)
