---
title: "[论文阅读笔记 -- 跨模态检索] Semantically Self-Aligned Network for T2I Person ReID (2021)"
date: 2021-09-13T13:18:31+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "retrieval", "ReID"]
draft: false
---

# 2107.12666v2 Semantically Self-Aligned Network for Text-to-Image Part-aware Person Re-identification (2021)

[开源代码传送门](https://github.com/zifyloo/SSAN)

![Fig 1](/images/2021/PRN97/1.png)

## 概述

### 自然语言描述的自由形式带来的两个问题
+ 同一张图像对应的描述可能非常不同
+ 对于身体部件的描述可能以任意顺序呈现  

### 现有方法的两个局限
+ 注意力模型计算复杂度高
+ 短语提取依赖外部模型

本文提出语义自对齐网络 (Semantically Self-Aligned Network, SSAN)。  

![Fig 2](/images/2021/PRN97/2.png)

## Backbone

### 视觉表征提取 

ResNet-50  

得到 \\(F \in \mathbb{R}^{H \times W \times C}\\)。  

### 文本表征提取

Bi-LSTM  

双向均值拼接得到 \\(E \in \mathbb{R}^{C \times n}\\)，\\(n\\) 为单词数。  

## 全局特征提取

对 \\(F\\) 用 Global Max Pooling (GMP)，对 \\(E\\) 用 Row-wise Max Pooling (RMP)，进而将特征用共享的 \\(1 \times 1\\) 卷积层投影到公共特征空间：  

$$v_{g} = W_{g} GMP(F),$$
$$t_{g} = W_{g} RMP(E).$$

可计算两个全局特征之间的余弦相似度：  

$$S_{g} = \frac{v_{g}^T t_{g}}{||v_{g}|| ||t_{g}||}.$$

## 部件级别特征提取

各部件分支包含两个模块：  

+ Part-specific Feature Learning (PFL) module
+ Part Relation Learning (PRL) module

![Fig 3](/images/2021/PRN97/3.png)

### Part-specific Feature Learning (PFL)

试图在不借助外部工具的情况下从原始的文本描述中提取部件级别的文本特征。  

引入单词注意力模块 (Word Attention Module, WAM) 推断单词-部件相关性，预测第 \\(i\\) 个单词属于第 \\(k\\) 个部件的概率：  

$$s_{i}^{k} = \sigma(W_{p}^{k} e_{i}).$$

进而调整对于第 \\(k\\) 个部件的 \\(E\\)：  

$$E_{k} = [s_{1}^k e_{1}, s_{2}^{k} e_{2}, \cdots, s_{n}^{k} e_{n}].$$

计算得到部件对应的视觉与文本特征：  

$$v_{l}^{k} = W_{l}^{k} GMP(F_{k}),$$ 
$$t_{l}^{k} = W_{l}^{k} RMP(E_{k}).$$ 

拼接 K 个特征后计算余弦相似度：  

$$S_{l} = \frac{v_{l}^T t_{l}}{||v_{l}|| ||t_{l}||}.$$

![Fig 4](/images/2021/PRN97/4.png)

### Part Relation Learning

对于水平均分图像的方法，一个短语可能涉及多个部件，有的短语可能描述的是部件之间的关系。  

本文引入多视角非局部网络 (multi-view non-local network, MV-NLN)。  

首先在一个通过多视角投影得到的共享嵌入空间中计算 \\(v_{l}^{k}\\) 和 \\(v_{l}^{i} (i \ne k)\\) 之间的相似度：  

$$S_{ki} = \frac{\theta_{k}(v_{l}^{k})^T \phi_{i}(v_{l}^{i})}{||\theta_{k}(v_{l}^{k})|| ||\phi_{i}(v_{l}^{i})||},$$

进而对于第 \\(k\\) 个视觉局部特征，加权聚合其余 \\(K - 1\\) 个特征：  

$$\alpha_{ki} = \frac{exp(S_{ki})}{\sum_{i = 1,i \ne k}^{K} exp(S_{ki})},$$
$$v_{l_{in}}^{k} = W_{\gamma}^{k}(\sum_{i = 1, i \ne k} \alpha_{ki}\phi_{i}(v_{l}^{i})).$$

最终可以求得部件级别视觉特征：  

$$v_{n}^{k} = W_{n}^{k}(v_{l}^{k} + v_{l_{in}}^{k}).$$

类似方式可以得到部件级别文本特征 \\(t_{n}\\)，其 MV-NLN 参数共享，同样计算余弦相似度：  

$$S_{n} = \frac{v_{n}^T t_{n}}{||v_{n}|| ||t_{n}||}.$$

![Fig 5](/images/2021/PRN97/5.png)

## 目标函数

引入 Compound Ranking (CR) Loss，同时包含强监督项与弱监督项。  

$$L_{cr} = max(\alpha_{1} - S(I_{p}, D_{p}) + S(I_{p}, D_{n}), 0) \\\\ + max(\alpha_{1} - S(I_{p}, D_{p}) + S(I_{n}, D_{p}), 0) \\\\ + \beta \cdot max(\alpha_{2} - S(I_{p}, D\_{p}^{'}) + S(I_{p}, D_{n}), 0) \\\\ + \beta \cdot max(\alpha_{2} - S(I_{p}, D_{p}^{'}) + S(I_{n}, D_{p}^{'}), 0).$$

对 \\(\alpha_{2}\\) 的值动态调整：  

$$\alpha_{2} = (\lambda + 1) \frac{\alpha_{1}}{2},$$
$$\lambda = min(\frac{S(I_{p}, D_{p}^{'})}{S(I_{p}, D_{p})}, 1).$$

此外还用到 Id Loss。  
