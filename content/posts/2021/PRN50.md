---
title: "[论文阅读笔记 -- 频域] Frequency learning for image classification (2020)"
date: 2021-07-16T21:04:14+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# 2006.15476 Frequency learning for image classification (2020)

## 核心问题

如果一个神经网络完全设计成在频域进行操作会怎么样？  

本文设计了 FreqNet 研究上述问题。  

## 本文方法

![Fig 1](/images/2021/PRN50/1.png)

### 图像分块

![Fig 2](/images/2021/PRN50/2.png)

对图像层级式地分块以提取多粒度信息。  

值得注意的一点是，即使只看很小的一块，也有可能识别出这一部分属于什么目标，本文方法探索这一可能性，因为其能够发现训练过程中应当被强调的相关频率。  

![Fig 3](/images/2021/PRN50/3.png)

### 2D DFT

![Fig 4](/images/2021/PRN50/4.png)

对各图像块 \\(i\\) 使用 2D DFT，各 \\(A_{i} \times B_{i}\\) 的块表示为 \\(f_{i}(x, y)\\)，DFT 计算如下：  

$$F_{i}(u, v) = \sum_{x=0}^{A_{i} - 1} \sum_{y=0}^{B_{i} - 1} f_{i}(x, y)e^{-j2 \pi (ux/A_{i} + vy/B_{i})}.$$  

![Fig 5](/images/2021/PRN50/5.png)

为了处理复数计算问题，如下得到各块对应谱的强度 (magnitude)：  

$$M_{i}(u, v) = \sqrt{(Real(F_{i}(u, v)))^2 + (Imag(F_{i}(u, v)))^2}.$$  

### 频率池化

![Fig 6](/images/2021/PRN50/6.png)

DFT 域中，频率从谱的中心沿着半径从低到高变化。在这种情况下，频率通常以径向方式进行滤波。  

因而，半径接近的频率成分可以被归为一组。  

除了半径频带之外，方形频率组 (squared frequency groups) 也是可能的选择，唯一区别是用 Chebyshev 距离而非欧几里德距离。  

![Fig 7](/images/2021/PRN50/7.png)

### 频率滤波

![Fig 8](/images/2021/PRN50/8.png)

用一个矩阵 \\(W_{i} \in R^2\\) 与池化后的频率强度做哈达玛积：  

$$\mu_{i} = M_{i} \odot W_{i}.$$  

![Fig 9](/images/2021/PRN50/9.png)

为了简化滤波过程，可以根据池化类型定义带权重的环或方形。  

接着对每个块计算一个系数：  

$$C_{i}(r) = \sum_{r=1}^{R} \mu_{i}(r).$$  

###全连接训练

![Fig 10](/images/2021/PRN50/10.png)

最终拼接所有 \\(C_{i}(r)\\)，作为全连接层的输入。  
