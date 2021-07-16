---
title: "[论文阅读笔记 -- 频域] Improving Image Classification With Frequency Domain Layers (MLSP 2017)"
date: 2021-07-16T15:02:02+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "CNN"]
draft: false
---

# Improving image classification with frequency domain layers for feature extraction (MLSP 2017)

![Fig 1](/images/2021/PRN48/1.png)

## 核心

研究从频域提取的特征对深度网络架构的作用。  

提出频率特征提取层。  

![Fig 2](/images/2021/PRN48/2.png)

## 方法

对输入图像层级式分块以提取多粒度信息，对各块使用 2D DFT。  

接着频率过滤作用于各块的幅度谱，用不同带宽进行过滤，从低频到高频指数上升。  

$$B_{k}(u, v) = \begin{cases} 1, \qquad \forall u, v \ | \ 2^{k-1} \le \sqrt{u^2 + v^2} < 2^k \\\\ 0, \qquad otherwise \end{cases},$$

其中 \\(k\\) 为带索引。  

![Fig 3](/images/2021/PRN48/3.png)

除此以外 Gaussian 以及 Butterworth 过滤等都可以使用。  

使用矩阵哈达玛积：  

$$R_{i,k}(u, v) = F_{i}(u, v) \odot B_{k}(u, v).$$  

进而就是下采样与 FC。  
