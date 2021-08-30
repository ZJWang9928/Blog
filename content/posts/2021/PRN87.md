---
title: "[论文阅读笔记 -- 频域] Learning Omni-frequency Region-adaptive Representations (AAAI 2021)"
date: 2021-08-30T11:13:40+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# Learning Omni-frequency Region-adaptive Representations for Real Image Super-Resolution (AAAI 2021)

![Fig 1](/images/2021/PRN87/1.png)

## 概述

本文提出 Omni-frequency Region-adaptive Network (ORNet) 解决真实图像超分问题。  

## 模型架构

![Fig 2](/images/2021/PRN87/2.png)

## 频率分解模块 (Frequency Decomposition Module, FD)

FD 模块由两个阶段组成：  
+ 频率分解阶段
+ 频率增强阶段

频率分解阶段将 LR 输入分解为高/中/低频成分，频率强化阶段强化各表征。  

### 频率分解阶段

$$f_{l} = Conv\downarrow_{2}(Conv\downarrow_{2}(I)),$$
$$f_{m} = Conv\downarrow_{2}(I) - Conv\downarrow_{2}(Conv\downarrow_{2}(I))\uparrow_{2},$$
$$f_{h} = Conv(I) - Conv\downarrow_{2}(I)\uparrow_{2},$$

其中 \\(Conv\downarrow_{2}\\) 表示步长为 \\(2\\) 的卷积层，\\(Conv\\) 表示不带下采样的卷积层，\\(\uparrow\\) 表示双线性上采样操作。  

### 频率增强阶段

用到 Frequency Enhancement Unit (FEU)。  

$$\widetilde{f}\_{l} = Enhance(f_{l}),$$
$$\widetilde{f}\_{m} = Enhance(f_{m}, \widetilde{f}\_{l}),$$
$$\widetilde{f}\_{h} = Enhance(f_{h}, \widetilde{f}\_{m}, \widetilde{f}\_{l}),$$

其中 \\(Enhance(\cdot)\\) 表示若干个 FEU。  

![Fig 3](/images/2021/PRN87/3.png)

## 区域自适应频率聚合模块 (Region-adaptive Frequency Aggregation Module, RFA)

![Fig 4](/images/2021/PRN87/4.png)
