---
title: "[论文阅读笔记 -- 注意力机制] Attention in Attention Network for Image Super-Resolution (2021)"
date: 2021-08-10T11:12:17+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention"]
draft: false
---

# 2104.09497 Attention in Attention Network for Image Super-Resolution (2021)

[开源代码传送门](https://github.com/haoyuc/A2N)

![Fig 1](/images/2021/PRN75/1.png)

## 概览

图像超分任务。  

提出一种 attention in attention 块 (\\(A^2B\\))，并设计了 \\(A^2N\\) 架构。  

## 出发点

### 两个问题
+ 图像的哪一部分倾向于有更高或更低的注意力系数？
+ 注意力机制是否总对超分模型有利？

## 网络架构

![Fig 2](/images/2021/PRN75/2.png)

## \\(A^2B\\)

![Fig 4](/images/2021/PRN75/4.png)

### 动态的注意力 Dropout 机制

![Fig 3](/images/2021/PRN75/3.png)
