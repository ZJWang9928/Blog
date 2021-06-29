---
title: "[论文阅读笔记 -- 文本对抗样本] Seq2Sick: Evaluating Robustness of Seq2Seq Models (AAAI 2020)"
date: 2021-06-29T21:36:09+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "nlp", "notes", "adversarial", "GAN", "adversarial samples"]
draft: false
---

# Seq2Sick: Evaluating the Robustness of Sequence-to-Sequence Models with Adversarial Examples (AAAI 2020)

## 背景

对抗攻击可用于衡量 DNN 的鲁棒性，对抗样本越容易生成则模型越健壮。  

攻击图像比攻击文本容易得多，因为图像空间是连续的，在累积失真 (accumulated distortion) 很小的情况下即使改变大多数像素，扰动也不易被人察觉。而在离散的文本空间上，单词级别的修改可能严重改变文本的含义，因而应当改变尽可能少的单词。    

同时，攻击一个分类器也比攻击序列化输出的模型容易，原因在于输出的搜索空间大小差异。  

本文研究为 seq2seq 问题构建对抗样本。主要研究两个问题：  

+ 是否有可能轻微改变 seq2seq 模型的输入就严重改变其输出？  
+ seq2seq 模型是否比基于 CNN 的图像分类器更加健壮？  

## 问题建模

seq2seq 上对抗样本的定义可以建模为如下优化问题：  

$$min_{\delta} L(X + \delta) + \lambda \cdot R(\delta),$$  

后一项控制扰动的大小，前一项可以是不同形式的损失函数。  

## 本文关注的两种攻击

### 无重合攻击 (non-overlapping attack)

要求输出与原始输出没有重合，比无目标攻击只要求与原始输出不同更加困难。  

### 目标关键词攻击 (targeted keywords attack)

要求找到一个对抗输入序列，使得所有目标关键词必须出现在其对应的输出中，比无重合攻击更加困难。  

## 处理离散的输入空间

### 一种简单方法

首先在连续空间通过解前述优化问题，学习 \\(X + \delta^{\*}\\)，进而在单词嵌入集合 \\(\mathbb{W}\\) 中搜索其最接近的词嵌入。由于维度灾难问题，这个方法在目标关键词攻击上表现极差。  

为了解决离散输入空间问题，本文增加了一个额外的约束，要求 \\(X + \delta\\) 属于属于词典 \\(\mathbb{W}\\)：  

$$min_{\delta} L(X + \delta) + \lambda \cdot R(\delta),$$
$$s.t. \quad x_{i} + \delta_{i} \in \mathbb{W} \quad \forall i = 1, \cdots, N$$

接着使用投影梯度下降 (projected gradient descent) 来解这个约束优化问题。在每步迭代将当前解 \\(x_{i} + \delta_{i}\\) 投影回 \\(\mathbb{W}\\) 以确保 \\(X + \delta\\) 能够映射到一个特定的输入词。  

### Group Lasso 正则化

广泛使用的 \\(\mathcal{l}\_{2}\\) 范数 \\(||\delta||\_{2}^2\\) 并不适用于本任务，因为使用 \\(\mathcal{l}\_{2}\\) 正则化学习到的扰动 \\(\\{\delta\_{t}\\}\_{t=1}^M\\) 是非零的，由此大多数输入的单词会被扰动到另一个词，导致对抗序列与原始输入差异巨大。  

为了解决这一问题，本文将各个包含 \\(d\\) 个变量的 \\(\delta_{t}\\) 处理为一组，使用 group lasso 正则化来保证组稀疏性 (group sparsity)，即在最优解 \\(\delta^{\*}\\) 中只有少数组 (单词) 允许非零：  

$$R(\delta) = \sum\_{t=1}^N ||\delta_{t}||\_{2}.$$  

### 梯度正则化

攻击 seq2seq 模型时容易出现对抗样本集中于一个只有很少或没有嵌入向量的区域，这会影响投影梯度下降的表现，因为这些区域中最近的嵌入都很远。  

为了解决这一问题，本文采用梯度正则化，拉近 \\(X + \delta\\) 到单词嵌入空间的距离。最终的目标函数变成了：  

$$min\_{\delta} L(X + \delta) + \lambda_{1} \sum_{i=1}^N ||\delta_{i}||\_{2} + \lambda_{2} \sum_{i=1}^N min_{w_{j} \in \mathbb{W}} \\{||x_{i} + \delta_{i} - w_{j}||\_{2}\\}$$
$$s.t. \quad x_{i} + \delta_{i} \in \mathbb{W} \quad \forall i = 1, \cdots, N$$

第三项就是梯度正则化项。具体优化过程如下：  

![Alg 1](/images/2021/PRN27/A1.png)

## 一些例子

![Tab 7](/images/2021/PRN27/T7.png)

![Tab 8](/images/2021/PRN27/T8.png)

![Tab 9](/images/2021/PRN27/T9.png)
