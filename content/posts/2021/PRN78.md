---
title: "[论文阅读笔记 -- 频域] Phase Consistent Ecological Domain Adaptation (CVPR 2020)"
date: 2021-08-15T12:28:20+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# Phase Consistent Ecological Domain Adaptation (CVPR 2020)

[开源代码传送门](https://github.com/donglao/PCEDA)

![Fig 1](/images/2021/PRN78/1.png)

## 概述

无监督域适应 (Unsupervised Domain Adaptation, UDA) 任务。  

### 通常的 UDA 方法
+ 学习一个从源分布到目标分布的映射
+ 训练一个对域变化不敏感的骨架网络

本文引入两个约束，一个作用于域间映射，一个作用于目标域中的分类器。  

语义信息倾向于与傅里叶变换的相位有关，幅度改变能显著改变外观，但是不改变图像判读。因而保持相位一致性有助于提升表现。  

![Fig 2](/images/2021/PRN78/2.png)

### 相位一致性

\\(\mathcal{F}: \mathbb{R}^{H \times W} \rightarrow \mathbb{R}^{H \times W \times 2}\\) 为傅利叶变换，对于一个变换 \\(T\\) 以及一张单通道图像 \\(x\\) 通过最小化下式保持相位一致性：  

$$\mathcal{L}\_{ph}(T; x) = -\sum_{j}\frac{<\mathcal{F}(x)\_{j}, \mathcal{F}(T(x))\_{j}>}{||\mathcal{F}(x)\_{j}||\_{2} \cdot ||\mathcal{F}(T(x))\_{j}||\_{2}},$$

即为原始与变换后相位间差异的负余弦值。  

## 模型架构

![Fig 3](/images/2021/PRN78/3.png)
