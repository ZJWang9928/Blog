---
title: "[论文阅读笔记 -- 图像标注] Unsupervised Image Captioning (CVPR 2019)"
date: 2021-09-22T13:14:16+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "image captioning", "unsupervised"]
draft: false
---

# Unsupervised Image Captioning (CVPR 2019)

[开源代码传送门](https://github.com/fengyang0317/unsupervised_captioning)

![Fig 1](/images/2021/PRN99/1.png)

## 概述

无监督图像标注任务，即不使用任何标记好的图文对来训练图像标注模型。  

### 三个目标
+ 用文本对抗生成方法，在句子语料上训练一个语言模型，其能够基于所给的图像特征生成句子。由于没有标签，使用对抗方法使生成的句子无法与语料句子相区别。  
+ 将视觉概念检测器的知识蒸馏到图像标注模型中。当生成的句子中出现与图像的视觉概念相应的单词时，予以激励。  
+ 将图像与句子投影到一个公共潜在空间中。  

![Fig 2](/images/2021/PRN99/2.png)

## 主要部件

### 图像编码器

Inception-V4  

### 句子生成器

LSTM  

### 句子鉴别器

$$[q_{t}, h_{t}^{d}] = LSTM^{d}(x_{t}, h_{t-1}^{d}), \ t \in \\{1, \cdots, n\\}.$$

## 训练

### 对抗标注生成

生成器以图像特征作为数据，以此为根据生成一个句子。  

鉴别器区分生成的句子与语料中的句子。  

在每个时间步计算一个对抗激励：  

$$r_{t}^{adv} = log(q_{t}),$$
$$\mathcal{L}\_{adv} = -[\frac{1}{l} \sum_{t = 1}^{l} log(\hat{q}\_{t}) + \frac{1}{n} \sum_{t = 1}^{n} log(1 - q_{t})],$$  

\\(l\\) 是预料句子长度，\\(n\\) 为生成的句子长度。  

### 视觉概念蒸馏

各时间步计算概念激励：  

$$r_{t}^{c} = \sum_{i = 1}^{N_{c}} I(s_{t} = c_{i}) \* v_{i}.$$

### 双向图文重构

将图文特征投影到一个公共潜在空间中，进而互相重构，使二者在语义上一致。  

![Fig 3](/images/2021/PRN99/3.png)

#### 图像重构

重构图像特征而非完整图像。  

用于训练鉴别器的图像重构损失为：  

$$\mathcal{L}\_{im} = ||x_{-1} - x'||\_{2}^{2}.$$

取负为生成器的图像重构激励：  

$$r_{t}^{im} = \mathcal{L}\_{im}.$$

#### 文本重构

$$\mathcal{L}\_{sen} = -\sum_{t = 1}^{l} log(p(s_{t} = \hat{s}\_{t} | \hat{s}\_{1}, \cdots, \hat{s}\_{t - 1})).$$
