---
title: "[论文阅读笔记 -- 频域 / Transformer] FNet: Mixing Tokens with Fourier Transforms (2021)"
date: 2021-07-10T15:34:14+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "frequency", "nlp"]
draft: false
---

# 2105.03824 FNet: Mixing Tokens with Fourier Transforms (2021)

[开源代码传送门](https://github.com/google-research/google-research/tree/master/f_net)

## 出发点

用更简单的 token 混合机制取代自注意力层。  

最终选择傅利叶变换，设计了 FNet 模型。  

## 离散傅利叶变换 (Discrete Fourier Transform, DFT)

傅利叶变换将一个函数分解频域成分，对于一个序列 \\(\\{x_{n}\\}, n \in [0, N-1]\\)，DFT 定义为：  

$$X_{k} = \sum_{n=0}^{N-1} x_{n} e^{-\frac{2\pi i}{N}nk}, \quad 0 \le k \le N-1.$$  

对于各个 \\(k\\)，DFT 通过对全部的原始输入 token \\(x_{n}\\) 根据旋转因子 (twiddle factors) 加权求和，生成一个新的特征 \\(X_{k}\\)。  

计算 DFT 有两种主要的方法，快速傅利叶变换 (Fast Fourier Transform, FFT) 与矩阵乘法。  

标准的 FFT 算法是 Cooley-Tukey 算法，复杂度为 \\(\mathcal{O}(NlogN)\\)。  

一种替代方式是将 DFT 矩阵简单作用于输入序列。DFT 矩阵是如下的范德蒙矩阵：  

$$W_{nk} = (e^{-\frac{2\pi i}{N}nk} / \sqrt{N}), \quad n,k = 0, \cdots, N-1.$$  

复杂度为 \\(\mathcal{O}(N^2)\\)，但是对于较短序列在 TPU 上计算更快。  

![Fig 1](/images/2021/PRN38/1.png)

## FNet 架构

将各 Transformer 编码层中的自注意力子层替换为傅利叶子层，其对输入的 \\((sequence \ length, hidden \ dimension)\\) 嵌入做 2D DFT，两个维度各一个 1D DFT：  

$$y = \mathcal{R}(\mathcal{F}\_{seq}(\mathcal{F}\_{h}(x))).$$  

只保留实数部分，从而无需修改前向传播层来处理复数，实验发现只对最后整体的变换结果取实部得到的结果最好，即  

$$y = \mathcal{R}(\mathcal{F}\_{seq}(\mathcal{R}(\mathcal{F}\_{h}(x))))$$  

会使得 FNet 精度下降且稳定性变低。  

可将交替的编码块视为交替使用正反傅里叶变化，即将输入在时域与频域之间来回变换。  
