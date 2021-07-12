---
title: "[论文阅读笔记 -- 图像去噪] NBNet: Noise Basis Learning with Subspace Projection (CVPR 2021)"
date: 2021-07-12T21:56:05+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "image deniosing"]
draft: false
---

# 2012.15028 NBNet: Noise Basis Learning for Image Denoising with Subspace Projection (CVPR 2021)

![Fig 1](/images/2021/PRN44/1.png)

## 核心思想

通过图像投影，利用非局部的图像信息。  

由输入图像生成一组图像基向量 (image basis vectors)，接着在这些基向量张成的子空间中重构图像。  

![Fig 2](/images/2021/PRN44/2.png)

## 基的生成

$$V = [v_{1}, v_{2}, \cdots, v_{K}] = f_{theta}(X_{1}, X_{2}).$$

## 投影重构

通过正交线性投影 (orthogonal linear projection) 将特征图 \\(X_{1}\\) 投影到 \\(V\\) 长成的子空间 \\(\mathcal{V}\\) 中。  

正交投影矩阵 \\(P: \mathbb{R}^{N} \rightarrow \mathcal{V}\\) 可由 \\(V\\) 计算得到：  

$$P = V(V^TV)^{-1}V^T.$$  

正则化项 \\((V^TV)\\) 确保基向量互相正交。  

最终将图像特征图 \\(X_{1}\\) 重构到信号子空间：  

$$Y = PX_{1}.$$  
