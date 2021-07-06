---
title: "[论文阅读笔记 -- 对抗样本] Query Attack via Opposite-Direction Feature (2018)"
date: 2021-07-06T15:13:34+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "adversarial", "GAN", "adversarial samples", "retrieval", "cross-modal", "reid"]
draft: false
---

# 1809.02681 Query Attack via Opposite-Direction Feature:Towards Robust Image Retrieval (2018)

![Fig 1](/images/2021/PRN33/1.png)

## 背景

### 现有分类攻击方法在在检索场景中的困难

+ 其目标是类别预测，与检索任务不同
+ 检索场景中训练时的类别与测试时通常是不同的  

本文针对图像检索任务生成对抗样本。  

### 攻击的两种选择

+ 攻击 query 图像
+ 攻击数据库中图像

本文主要针对前者。  

设计了一种白盒攻击方法，称为反向特征攻击 (Opposite-Direction Feature Attack, ODFA)，核心想法在于显式地使对抗样本的特征远离其原始特征。  

![Fig 2](/images/2021/PRN33/2.png)

## 反向特征攻击 (ODFA)

定义如下损失函数：  

$$argmin_{X'} J(X') = (\frac{f_{X'}}{||f_{X'}||\_{2}} + \frac{f_{X}}{||f_{X}||\_{2}})^2.$$  

旨在使得对抗样本 \\(X'\\) 的特征处于原始样本特征的对面一侧，称 \\(-f_{X}\\) 为反向特征。  

具体算法描述如下：  

![Alg 1](/images/2021/PRN33/A1.png)

## 针对多尺度输入的反向特征攻击 (ODFA-MS)

具体算法描述如下：  

![Alg 2](/images/2021/PRN33/A2.png)
