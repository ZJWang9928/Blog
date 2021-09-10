---
title: "[论文阅读笔记 -- ReID] Parameter-Efficient Person Re-identification in the 3D Space (2020)"
date: 2021-09-10T15:44:37+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "3D", "reid", "retrieval"]
draft: false
---

# 2006.04569 Parameter-Efficient Person Re-identification in the 3D Space (2020)

[开源代码传送门](https://github.com/layumi/person-reid-3d)

![Fig 1](/images/2021/PRN96/1.png)

![Fig 2](/images/2021/PRN96/2.png)

## 概述

考察 2D 行人外观与 3D 几何结构之间的互补信息。  

本文提出 Omni-scale Graph Network (OG-Net)，在 3D 空间中进行 ReID。  

![Fig 3](/images/2021/PRN96/3.png)

## 模型架构

首先用预训练模型将 2D 图像映射到 3D 空间，得到 3D 点云集合 \\(S = \\{s_{n}\\}\_{n=1}^{N}\\)，点云 \\(s_{n} \in \mathbb{R}^{m \times 6}\\)，\\(m\\) 个点 \\(6\\) 个通道，前 \\(3\\) 通道包含 3D 坐标 XYZ，后 \\(3\\) 通道包含相应 RGB 信息。  

由于 3D 点云不同于传统的图像格式，是无序且离散的，无法直接将传统的 2D 卷积层作用于 \\(m \times 6\\) 的点云，本文基于点之间距离构建图 \\(\mathcal{G}\\)，采用动态图卷积来学习 \\(\mathcal{G}\\)。  

### 动态图卷积

采用 KNN 图建模邻居点之间的关系，有向并有自环。
