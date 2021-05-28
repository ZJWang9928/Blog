---
title: "[论文阅读笔记 -- VQA] Learnable Aggregating Net with Diversity Learning (MM 2019)"
date: 2021-05-28T19:43:09+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Learnable Aggregating Net with Diversity Learning for Video Question Answering (MM 2019)

## V-VQA 三个难点
+ 视频通常包含大量冗余信息
+ 一些视频相关问题涉及多个关键帧，较难定位
+ 有效聚合视频与句子特征以捕捉回答真实分布的方法很少被研究

## 现有 V-VQA 方法
+ 只关注为外观特征、时域特征或是拼接得到的空间-时域特征构建视觉特征图
+ 并没有协同地对来自视频和问题的内容及上下文信息进行推理

## 解决两个核心问题
+ 怎样基于视频复杂而多样化的内容使用 co-attention (与 [STA](http://jonathanwayy.xyz/2021/prn4/) 所用注意力类似)，将其从 I-VQA 拓展到 V-VQA
+ 怎样在不破坏特征分布与时域信息的前提下汇聚帧级别的特征

## 导致 Co-attention 无法直接用于 V-VQA 的两个缺陷
+ 只有单个变换路径的 co-attention 缺乏多样性，尤其是视频不定长时无法捕捉到独特、互补的特征，需要多路注意力特征图 (multi-path attention maps) 以去除无关 clip、提升相关 clip 的权重并在回答时定位特定的 clip
+ 现有的 co-attention 使用常见的特征聚合方法，如加权和等，可能会改变特征分布，进而导致语义上下文信息的损失；此外基于递归的聚合方式或许能提供解决方案，但是计算量往往过大

因此，需要有一种非递归的聚合方式来为最终的答案预测选取线索，本文提出 LAD-Net，包含两个主要部件  
+ multi-path pyramid co-attention (MPC) with a diversity learning 用于生成注意力图以捕捉上下文多样性
+ learnable aggregation modular with evidence-gating

![Fig 1](/images/2021/PRN5/1.png)

## Multi-path Pyramid Co-attention with Diversity Learning

### Multi-path Pyramid Co-Attention (MPC)
传统的 co-attention 机制需要一个有单路径特征变换的注意力生成器，或一组独立的在单一尺度条件下进行多路线性投影的注意力生成器。最终回到这所生成的注意力图具有相似的数据分布，缺乏多样性。 

因此 MPC 模块旨在将单路变多路，首先将输入的 V 或 Q 变换到 H 个子空间，以形成特征金字塔结构，促进多样性，这样一来就有 H 种 V 和 Q 之间的相似度矩阵，第 i 个相似度矩阵可以写作 \\[S_{i} = QW_{wqi}(VW_{wvi})^{T}.\\]  

后续各相似度矩阵的处理与[通常做法](http://jonathanwayy.xyz/2021/prn4/)类似，沿两个方向归一化得到两种注意力图，进而得到处理后的注意力特征 \\(V_{att}^{i}\\) 和 \\(Q_{att}^{i}\\)，最终整合的文本与视频特征可由所有子空间的特征求和得到 \\[Q_{att}^{f} = Q + \sum Q_{att}^{i},\\] \\[V_{att}^{f} = V + \sum V_{att}^{i}.\\]  

<!-- 最终，输出的各个尺度的注意力与最初的特征融合得到 weighted visual features 和 weighted word features。   -->

### 多样性学习 (Diversity Learning)
上一节方法从输入层面促进多样性，本节方法从输出层面进一步巩固多样性。  

在两个模态各自引入一个多样性学习目标函数，来惩罚具有较高相似度分数的注意力处理后的特征，用余弦相似度作为衡量标准。  

即使得同模态特征之间相似度不能过高。  

## Feature Learnable Aggregating

## Evidence-Gating

## 回答推断
与前文类似。  
