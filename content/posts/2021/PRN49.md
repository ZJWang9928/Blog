---
title: "[论文阅读笔记 -- 频域] Global Filter Networks for Image Classification (2021)"
date: 2021-07-16T19:50:39+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image", "attention", "ViT"]
draft: false
---

# 2107.00645 Global Filter Networks for Image Classification (2021)

[开源代码传送门](https://github.com/raoyongming/GFNet)

![Fig 1](/images/2021/PRN49/1.png)

## 核心方法

本文提出一种全局滤波器网络 (Global Filter Network, GFNet)，在频域中学习空间位置之间的相互关系。

不同于视觉 transformer 中的自注意力机制与 MLP 模型中的全连接层，token 之间的相互关系建模为一组可学习的全局滤波器 (global filters)，作用于输入特征的谱。  

由于全局滤波器能够覆盖所有频率，模型可以捕捉长距离与短距离的相互关系。  

![Alg 1](/images/2021/PRN49/A1.png)

### 用三个部件替代 ViT 的自注意力层
+ 2D DFT
+ 频域特征与全局滤波器之间的哈达玛积
+ 2D IDFT  

## 全局滤波器层

全局滤波器 \\(K\\) 可以视为一组可学习的针对不同隐藏维度的频率滤波器。  

可以证明全局滤波器层等价于滤波器尺寸为 \\(H \times W\\) 的面向深度的全局循环卷积 (depthwise global circular convolution)。  

因而全局滤波器层与标准卷积层采用相对较小滤波器增强局部归纳偏差 (enforce the inductive biases of the locality) 不同。  

## GFNet 与 [FNet](http://jonathanwayy.xyz/2021/prn38/) 对比

+ FNet 对输入做 FFT 并直接将谱的实部加到输入 token 上，这导致不同域的信息混合在一起；GFNet 从频率滤波器获得启发，更合理
+ FNet 只保留谱的实部，而由于实序列输入的谱是共轭对称的，实部对称因而有冗余信息；GFNet 利用这一特性简化运算
+ FNet 针对 NLP 任务；GFNet 针对视觉任务  

## 两种变体

+ 每个块具有固定数量 token 的 transformer 形式模型
+ 具有逐步下采样 token 的 CNN 形式层叠式模型  

## 全局滤波器可视化

![Fig 2](/images/2021/PRN49/4.png)
