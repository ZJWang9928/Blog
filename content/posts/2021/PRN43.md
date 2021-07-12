---
title: "[论文阅读笔记 -- 注意力机制] Non-Local Neural Networks (CVPR 2018)"
date: 2021-07-12T15:09:26+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention"]
draft: false
---

# Non-Local Neural Networks (CVPR 2018)

[开源代码传送门](https://github.com/facebookresearch/video-nonlocal-net)

![Fig 1](/images/2021/PRN43/1.png)

## 核心内容

提出非局部操作用于捕捉长距离依赖关系，通过输入特征图所有位置上特征的加权和计算各位置的响应值 (response)。  

### 三个优势
+ 不同于局部渐进式的卷及或递归操作，非局部操作通过计算任意两个位置之间的交互关系直接捕捉长距离依赖关系
+ 高效，浅层也可以得到很好表现
+ 保留了可变化的输入尺寸，容易与卷积等其他操作结合

![Fig 2](/images/2021/PRN43/2.png)

## 定义

首先定义一个一般的非局部操作：  

$$y_{i} = \frac{1}{\mathcal{C}(x)} \sum_{\forall j}f(x_{i}, x_{j})g(x_{j}).$$  

非局部操作与全连接区别在于全连接使用学习好的参数，关系并非输入数据的函数，而非局部则基于位置之间的关系计算响应；此外尺度可变性也是一个差异。  

## 一些实例

文中为了简化，只考虑 \\(g\\) 为线性嵌入即 \\(g(x_{j}) = W_{g}x_{j}\\) 的形式，实现为 \\(1 \times 1\\) 或 \\(1 \times 1 \times 1\\) 卷积。  

对 \\(f\\) 的选取讨论了如下一些变体。  

### Gaussian

$$f(x_{i}, x_{j}) = e^{x_{j}^Tx_{j}},$$
$$\mathcal{C}(x) = \sum_{\forall j}f(x_{i}, x_{j}).$$

### Embedded Gaussian

$$f(x_{i}, x_{j}) = e^{\theta(x_{j})^T\theta(x_{j})},$$
$$\mathcal{C}(x) = \sum_{\forall j}f(x_{i}, x_{j}).$$

### 点乘

$$f(x_{i}, x_{j}) = \theta(x_{j})^T\theta(x_{j}),$$
$$\mathcal{C}(x) = N.$$

### 拼接

$$f(x_{i}, x_{j}) = ReLU(w_{f}^T[\theta(x_{i}), \phi(x_{j})]),$$
$$\mathcal{C}(x) = N.$$

## 非局部块

非局部块定义为  

$$z_{i} = W_{z}y_{i} + x_{i}.$$  

## 效果示例

![Fig 3](/images/2021/PRN43/3.png)
