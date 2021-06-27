---
title: "[论文阅读笔记 -- 对抗样本] Cross-Modal Learning with Adversarial Samples (NeurIPS 2019)"
date: 2021-06-27T13:29:36+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "adversarial", "GAN", "adversarial samples", "cross-modal"]
draft: false
---

# Cross-Modal Learning with Adversarial Samples (NeurIPS 2019)

![Fig 1](/images/2021/PRN23/1.png)

文中主要以跨模态哈希检索为例，其搜索空间大致可分为四个部分：T2T、I2I、I2T/T2I、NR (not relevant)。  

理想的针对跨模态任务的对抗样本应当不仅能够对跨模态检索作出有效攻击，还应该在进行同模态检索是表现不下降。  

本文设计了一种利用对抗样本的跨模态相关性学习方法 (Cross-Modal correlation Learning with Adversarial samples, CMLA)，是将对抗样本用于跨模态任务的首个工作。  

![Fig 2](/images/2021/PRN23/2.png)

## CMLA 模型架构

对于一个图像数据点 \\(o^{v}\\)，意在通过学习一个小的扰动 \\(\delta^{v}\\)，生成一个对抗样本 \\(\hat{o}^{v} = o^{v} + \delta^{v}\\)。将该对抗样本传入跨模态哈希网络，希望能够得到失配的文本模态数据。  

为实现这一目标，首先由原始的配对的文本数据得到一个常规哈希编码 (regular hash codes) \\(H^t = \mathcal{H}^t(o^t, \theta^t)\\)。对抗样本学习问题可以转化为最大化两个跨模态哈希编码之间汉明距离的问题。  

为此引入一个模态间相似度正则化 (inter-modal similarity regularization)，对应的模态间相似度损失函数定义如下：  

$$min_{\delta^v} \mathcal{J}_{inter}^v = \sum_{i,j=1}^N(log(1 + e^{-\Gamma_{ij}}) + S_{ij}\Gamma_{ij}) + \sum_{i=1}^N ||\hat{o}_{i}^v - o_{i}^v||_{p},$$
$$\Gamma_{ij} = \frac{1}{2}(\hat{H}_{i}^v)(H_{j}^t)^T.$$  

在为跨模态数据学习对抗样本的过程中，各模态的内部相关性应当被保留。为此又引入了一个模态内相似度正则化：  

$$min_{\delta^v} \mathcal{J}_{intra}^v = \sum_{i,j=1}^N(log(1 + e^{-\Theta_{ij}}) + S_{ij}\Theta_{ij}),$$
$$\Theta_{ij} = \frac{1}{2}(\hat{H}_{i}^v)(H_{j}^v)^T.$$  

文本模态对抗样本的学习与之相似。  

为了学习对抗样本，网络参数固定后优化 \\(\delta^v\\) 和 \\(\delta^t\\)，具体的 CMLA 学习过程如下。  

![Fig A1](/images/2021/PRN23/A1.png)

## 一些学习到的对抗样本示例

![Fig 3](/images/2021/PRN23/3.png)

图像中上一排为原始图像，下一排为相应的对抗样本。文本有原始的词袋向量混入学习到的扰动后产生。  

## 模态内相似度正则化的作用

![Fig 4](/images/2021/PRN23/4.png)
