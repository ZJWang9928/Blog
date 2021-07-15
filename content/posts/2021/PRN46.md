---
title: "[论文阅读笔记 -- 频域 / 人脸伪造检测] Spatial-Phase Shallow Learning (CVPR 2021)"
date: 2021-07-15T21:12:47+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "face forgery", "detection"]
draft: false
---

# Spatial-Phase Shallow Learning: Rethinking Face Forgery Detection in Frequency Domain (CVPR 2021)

![Fig 1](/images/2021/PRN46/1.png)

## 背景

人脸伪造检测 (face forgery detection) 任务。  

在合成假脸过程中使用上采样，该操作通常会在频域留下痕迹。  

![Fig 2](/images/2021/PRN46/2.png)

### 核心方法与观点

本文提出一种空间-相位浅层学习 (Spatial-Phase Shallow Learning, SPSL) 方法，利用相位谱 (phase spectrum) 检测常见的人造痕迹，其核心观点在于比起幅度谱 (amplitude spectrum)，相位谱对上采样操作更加敏感，可从图 1 与图 2 中看出。  

此外一个观点是在人脸伪造检测任务中，局部的纹理信息比高级语义信息更加重要，高级语义信息甚至应当被抑制，SPSL 丢弃了许多卷积层以减小感受野，从而迫使 CNN 更多去关注局部区域。  

![Fig 3](/images/2021/PRN46/3.png)

## SPSL

### Motivation

典型的面部操作方法分为三步：  

+ 源面部编码
+ 潜在空间中面部交换
+ 目标面部解码

从频域重构相位谱的空间域表征，即对相位谱做 IDFT 而不带幅度谱，最终将其与 RGB 图像沿着通道维拼接，得到一张 4 通道的 RGBP 图像。  

本文具体实现为 pristine phase spectrum 绝对值的 IDFT。  

## 特征可视化

![Fig 4](/images/2021/PRN46/4.png)
