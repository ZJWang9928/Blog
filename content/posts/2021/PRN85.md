---
title: "[论文阅读笔记 -- 频域] Frequency Separation for Real-World Super-Resolution (ICCVW 2019)"
date: 2021-08-27T13:40:16+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# 1911.07850 Frequency Separation for Real-World Super-Resolution (ICCVW 2019)

[开源代码传送门](https://github.com/ManuelFritsche/real-world-sr)

![Fig 1](/images/2021/PRN85/1.png)

## 速览

速读一篇文献，主要关注一下频域相关的处理。  

下采样操作去除了高频信息而保留低频信息。

因而本文对高低频信息进行了分离，进而分别处理，使用简单的线性滤波器实现，定义一个低通滤波器 \\(w_{l, d}\\)，通过卷积操作得到高低频信息：  

$$x_{L, d} = w_{L, d} \* x_{d},$$
$$x_{H, d} = x_{d} - x_{L, d} = (\delta - w_{L, d}) \* x_{d}.$$

故此高通滤波器定义为 \\(w_{H, d} = \delta - w_{L, d}\\)。  

![Fig 2](/images/2021/PRN85/2.png)

## 模型架构

![Fig 3](/images/2021/PRN85/3.png)

![Fig 4](/images/2021/PRN85/4.png)
