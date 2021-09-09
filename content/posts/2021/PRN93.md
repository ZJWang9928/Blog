---
title: "[论文阅读笔记 -- 跨模态检索] Modality-Specific and Shared GAN (PR 2020)"
date: 2021-09-09T21:42:02+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "adversarial"]
draft: false
---

# Modality-Specific and Shared Generative Adversarial Network for Cross-modal Retrieval (PR 2020)

![Fig 1](/images/2021/PRN93/1.png)

## 概述

本文提出 Modality-Specific and Shared Generative Adversarial Network (\\(MS^2GAN\\))。  

## 生成模型

### 标签预测

将模态中特征与公共空间特征拼接后用于预测标签，采用语义鉴别损失 (Semantic Discrimination Loss)，可视为分类任务。  

### 模态内间语义相似度建模

用欧几里德距离衡量一对实例 \\(o_{u}\\) 与 \\(o_{v}\\) 特征组之间的距离：  

$$d_{c}(u, v) = ||i_{u}^f - i_{v}^f||\_{2}^{2} + ||t_{u}^f - t_{v}^f||\_{2}^{2} + ||i_{u}^s - i_{v}^s||\_{2}^{2}.$$

对比损失 (Contrasive Loss) 实现为三元组损失，同时处理模态内间相似度。  

### 模态特殊与共享特征鉴别

$$d_{lm}^{i}(u, u) = ||i_{u}^{f} - i_{u}^{s}||\_{2}^{2},$$
$$d_{lm}^{t}(u, u) = ||t_{u}^{f} - t_{u}^{s}||\_{2}^{2},$$

对此使用三元组损失计算所谓的大边际损失 (Largin Margin Loss)。  

## 鉴别模型

两层子网络，鉴别模态。  

## 特征可视化

![Fig 4](/images/2021/PRN93/4.png)
