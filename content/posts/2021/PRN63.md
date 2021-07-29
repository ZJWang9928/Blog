---
title: "[论文阅读笔记 -- 跨模态检索] TIED: A Cycle Consistent Encoder-Decoder Model (CVPRW 2021)"
date: 2021-07-29T11:27:32+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "transformer", "cross-modal", "retrieval"]
draft: false
---

# TIED: A Cycle Consistent Encoder-Decoder Model for Text-to-Image Retrieval (CVPRW 2021)

![Fig 1](/images/2021/PRN63/1.png)

## 概览

Natural Language (NL) based Vehicle Track Retrieval 任务，对时间也有要求。  

本文提出一种文本到图像的编码器解码器网络 (Text-to-Image Encoder-Decoder network, TIED)，将图文映射到潜在空间，在将其联合映射回文本 query。  

![Fig 2](/images/2021/PRN63/2.png)

## 自然语言属性预处理

以半自动的方式从文本中提取标签，主要是颜色和车辆类型。

由于同样的属性可能会有不同词来描述，构建多标签属性 (multi-label attribute)，各图像配两个二值向量 \\(l_{c}\\) 和 \\(l_{t}\\)，这样一来两个词都标记为 \\(1\\) 即可。  

提取属性时，根据常见的连接词 (follow, behind, before, after) 分割句子，并只用第一部分，从而避免引入不相关的车辆特征。  

![Fig 3](/images/2021/PRN63/3.png)

## TIED

### 图像模型

ResNet50-IBN-a (2048-D) + FC (768-D) + BNNeck (512-D)  

用一个分类头 (FC) 生成标签预测。  

### 语言模型

编码器-解码器架构的预训练 BERT  

### 联合解码器

用 BERT decoder 将图文特征重构到文本空间。

## 目标函数

### 多标签分类损失

交叉熵进行属性分类  

$$\mathcal{L}\_{attr} = \sum_{i}^N t_{i} \cdot log(y_{i}),$$  

其中 \\(y_{i}\\) 是对属性 \\(i\\) 预测的标签，\\(t_{i}\\) 是目标标签。  

### 三元组损失

模态内、模态间均使用。  

### 循环损失 (Cycle Loss)

通过 BERT 解码器的输出预测 BERT 编码器的输入。  

## 结果示例

![Fig 4](/images/2021/PRN63/4.png)
