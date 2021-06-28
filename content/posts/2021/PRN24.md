---
title: "[论文阅读笔记 -- 带噪跨模态检索] Learning Cross-Modal Retrieval With Noisy Labels (CVPR 2021)"
date: 2021-06-28T11:45:52+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "noisy label", "retrieval", "cross-modal"]
draft: false
---

# Learning Cross-Modal Retrieval With Noisy Labels (CVPR 2021)

![Fig 2](/images/2021/PRN24/2.png)

## 背景

为了应对较高的数据标注成本，大规模数据会用到一些非专业的标注资源，从而不可避免地在标签中引入了噪音信息。  

由图 2 可以看出，噪声标签 (noisy labels) 会使得多模态学习在噪声数据集上过拟合，同时降低其在验证集上的表现。  

本文设计了一种多模态鲁棒学习框架 (Multimodal Robust Learning framework, MRL)，在最小化噪音样本影响的同时缩小异质鸿沟 (heterogeneous gap)。  

![Fig 1](/images/2021/PRN24/1.png)

提出了两种新的损失函数:  
+ 鲁棒聚类损失 (Robust Clustering loss, RC) \\(\mathcal{L}_{r}\\)，用于在不同模态之间从噪声标签更好地提取共享的具有鉴别能力的信息
+ 多模态对比损失 (Multimodal Contrastive loss, MC) \\(\mathcal{L}_{c}\\)，用于利用实例级别与配对级别的鉴别信息  

![Fig 3](/images/2021/PRN24/3.png)

## 鲁棒聚类分配 (Robust Clustering Assignment)

为了从噪声标签挖掘有效信息，首先将多模态数据聚类为 \\(K\\) 个公共类特定聚类分配 (common class-specific clustering assignment) \\(C = \\{c_{1}, \cdots, c_{K}\\}\\)，其中 \\(c_{k}\\) 是第 \\(k\\) 个类归一化厚的聚类中心向量 (clustering center vector)。对于一个样本 \\(x_{j}^i\\)，由对应模态网络得到 \\(z_{j}^i\\)， 其属于第 \\(k\\) 类的几率定义为  

$$p(k|x_{j}^i) = \frac{exp(\frac{1}{\tau_{1}}c_{k}^Tz_{j}^i)}{\sum_{t=1}^K exp(\frac{1}{\tau_{1}}c_{t}^Tz_{j}^i)}.$$

通过标准交叉熵损失函数 \\(\mathcal{L}_{CE}\\) 实现聚类：  

$$\mathcal{L}\_{CE} = -\frac{1}{N} \sum_{j=1}^N \sum_{k=1}^K q(k|x_{j}^i)log(k | x_{j}^i)),$$  

其中 \\(q(k | x_{j}^i)\\) 是不同标签的 ground-truth 几率。由图 3 可以看出，交叉熵倾向于重点优化更困难的样本，因为这些样本会产生更大的损失值，可以证明其能够在干净标注的标签 (clean-annotated labels) 上发挥作用。   

与交叉熵类似，传统的监督学习损失函数通常更关注较为困难的样本，而这些样本在含有噪声标签的情况下更可能是噪声样本，这导致了模型在噪声数据集上表现变差。  

为了减小噪声样本的影响，本对损失函数进行了修改，以降低困难样本的权重，同时提高简单样本的权重，使得模型在训练时更多关注干净样本而非噪声样本。  

与交叉熵损失不同，这里最小化负样本的对数似然，而不是正样本的对数似然的负值。RC 定义如下：  

$$\mathcal{L}\_{r} = \frac{1}{N} \sum_{i=1}^m \sum_{j=1}^N log(1-p(y_{j}^i | x_{j}^i)).$$  

值得注意的是 RC 的目标与如 Focal Loss 等的全监督损失 (fully supervised losses) 正好相反，那些损失主要关注的是不带噪声标签的干净数据集上的困难样本。  

由图 3 可以看出 RC 中干净样本能产生更大的损失值，从而主导梯度沿着正确的方向下降。  

## 多模态对比学习 (Multimodal Contrastive Learning)

通过在公共空间中最大化不同模态的一致性，学习公共特征的表征。  

将某个样本 \\(x_{j}^i\\) 属于 \\(m\\) 个模态中第 \\(j\\) 个实例的几率定义为：  

$$P(j|x_{j}^i) = \frac{\sum_{l=1}^m exp(\frac{1}{\tau_{2}}(z_{j}^l)^Tz_{j}^i)}{\sum_{l=1}^m \sum_{t=1}^N exp(\frac{1}{\tau_{2}}(z_{t}^l)^T z_{j}^i)}.$$

\\(P(j | x_{j}^i)\\) 也可以视为一个以 \\(x_{j}^i\\) 为中心的多模态聚类被正确识别为第 \\(j\\) 个聚类的几率。  

为了消除跨模态差异并挖掘实例级别的有效信息，用过最大化这些概率，使得来自同一实例的样本紧凑，而来自不同实例的样本分散。因而 MC 应最大化联合概率 \\(\prod_{i=1}^m \prod_{j=1}^N P(j | x_{j}^i)\\)，可以等价转化为最小化以下负的对数似然：  

$$\mathcal{L}\_{c} = -\frac{1}{N} \sum_{i=1}^m \sum_{j=1}^N log(P(j | x_{j}^i)).$$  

## 优化过程

![Alg 1](/images/2021/PRN24/A1.png)
