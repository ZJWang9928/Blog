---
title: "[论文阅读笔记 -- 跨模态检索] TIPCB: A Simple but Effective Part-based Convolutional Baseline"
date: 2021-06-15T13:42:56+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# 2105.11628 TIPCB: A Simple but Effective Part-based Convolutional Baseline for Text-based Person Search

![Fig 1](/images/2021/PRN11/1.png)

![Fig 2](/images/2021/PRN11/2.png)

## 视觉特征学习
在视觉 CNN 分支，ResNet-50 第 3 和第 4 个残差块的输出分别作为低级特征图和高级特征图。  
用 GMP 聚合低级特征图得到视觉的低级特征。对高级特征图使用 PCB (水平分割) 策略提取局部特征，取这些局部特征向量各个元素位置上的最大值构成全局特征向量。  
最终，得到 low-level、local-level 和 global-level 的三种特征。  
测试阶段只有 global-level 被用于相似度衡量。  

![Fig 3](/images/2021/PRN11/3.png)

## 文本特征学习
在文本 CNN 分支，使用预训练并固定参数的 BERT 提取词嵌入。  

为了满足卷积层输入的需要，将词嵌入维度由 \\(L \times D\\) 变为 \\(1 \times L \times D\\)。

设计了多分支的文本 CNN，共包含 K 个残差分支，对应行人图像的 K 个水平块。各个分支内包含 P 个文本参差瓶颈 (textual residual bottlenecks)，用于自适应地学习能够与视觉局部特征相匹配的文本特征。  

通过多分支文本 CNN 可以得到文本局部特征图，与视觉 CNN 分支类似，使用 GMP 提取文本局部特征并沿着通道维选取最大元素得到文本全局特征。  

## 多级跨模态匹配 (Multi-stage Cross-modal Matching)
对三级特征使用 Cross-Modal Projection Matching (CMPM) 损失。  

![Fig 4](/images/2021/PRN11/4.png)
