---
title: "[论文阅读笔记 -- ReID] PCNET: Parallelly Conquer Large Variance of Person ReId (ICIP 2021)"
date: 2021-09-10T13:22:48+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "reid", "retrieval"]
draft: false
---

# PCNET: Parallelly Conquer the Large Variance of Person Re-Identification (ICIP 2021)

![Fig 1](/images/2021/PRN95/1.png)

## 概述

本文提出 Parallelly Conquer Net (PCNet)，主要包含三个部件：  

+ Pose Adaptation Module (PAM)
+ Global Alignment Module (GAM)
+ Pixel-Wised Attention Module (PWAM)

用模块聚合单元 (Module Aggregation Unit) 整合各个子模块生成的特征。  

![Fig 2](/images/2021/PRN95/2.png)

## 模型架构

### Pose Adaptation Module (PAM)

用于缓解姿态差异问题。  

使用 Convolutional Pose Machines (CPM) 探测 14 个 human body landmark，用于得到 bounding box 将行人图像分割为不同区域。  

第 \\(i\\) 个区域的标记点集合记为 \\(\mathcal{S}\_{i} (i = 1, 2, 3)\\)，则其 bounding box \\(\mathcal{B}\_{i}\\) 为：  

$$\mathcal{B}\_{i} = [x_{i1}, y_{i1}, x_{i2}, y_{i2}] = [1, min_{j \in \mathcal{S}\_{i}}(y_{i}), W, max_{j \in \mathcal{S_{i}}}(y_{j})].$$

即用左上和右下点标定，宽度达到图像边界以减小标记点偏差的影响。  

### Global Alignment Module (GAM)

用于修正不对齐 (misalignment) 问题。  

human landmark 结合 STN 以学习用于对齐图像的变换矩阵。  

### Pixel-Wised Attention Module (PWAM)

用于缓解遮挡与背景等噪音信息的影响。  

较常规的注意力机制。  

### 模块聚合单元

带权拼接。  

\\(a_{0}\\) 为 baseline 的 Rank-1 准确率，\\(a_{1}, a_{2}, a_{3}\\) 为验证集上三个子模块的 Rank-1 准确率，则  

$$\alpha_{i} = \sqrt{\frac{a_{i} - a_{0}}{\sum_{j=1}^{3}(a_{j} - a_{0})}}, \ i = 1, 2, 3,$$
$$F_{final} = [\alpha_{1} f_{PAM}, \alpha_{2} f_{GAM}, \alpha_{3} f_{PWAM}].$$
