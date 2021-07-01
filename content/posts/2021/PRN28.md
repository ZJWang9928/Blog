---
title: "[论文阅读笔记 -- 遮挡 ReID] High-Order Information Matters (CVPR 2020)"
date: 2021-06-30T13:01:57+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid"]
draft: false
---

# High-Order Information Matters: Learning Relation and Topology for Occluded Person Re-Identification (CVPR 2020)

![Fig 1](/images/2021/PRN28/1.png)

## 背景

本文研究遮挡 ReID 问题 (Occluded Person Re-Identification)，该问题主要受到遮挡 (occlusion) 和出界 (outliers) 两个问题困扰。大部分现有方法只用到了一阶信息 (one-order information)。  

本文设计了一种新的架构，联合建模高阶关系 (high-order relation) 和行人拓扑信息 (human-topology information)，由一阶语义模块 \\(\mathcal{S}\\)、高阶关系模块 \\(\mathcal{R}\\) 以及高阶行人拓扑模块 \\(\mathcal{T}\\) 构成。  

![Fig 2](/images/2021/PRN28/2.png)

## 语义特征提取

用 CNN 和关键点模型分别提取特征图 \\(m_{cnn}\\) 和关键点热图 \\(m_{kp}\\)，通过外积和全局均值池化得到全局与局部特征：  

$$V_{l}^S = \\{v_{k}^S\\}\_{k=1}^K = g(m_{cnn} \otimes m_{kp}),$$
$$V_{g}^S = v_{K+1}^S = g(m_{cnn}).$$  

### 训练损失
分类损失 + 三元组损失。  

## 高阶关系学习

### 一个困难
遮挡 ReID 中许多遮挡区域的特征是无意义的，甚至是噪声信息，直接在图中传播会带来更多噪声。  

![Fig 3](/images/2021/PRN28/3.png)

### 方向自适应的图卷积层
为解决上述问题，本文设计了一种方向自适应的图卷积层 (adaptive-direction graph convolutional layer, ADGC) 来动态学习信息传递的方向和度。  

一个简单的图卷积层接受两个输入，一个邻接矩阵 \\(A\\) 和所有节点的特征 \\(X\\)，输出可以如下计算得到：  

$$O = \hat{A}XW,$$  

其中 \\(\hat{A}\\) 是 \\(A\\) 的归一化版本。通过基于输入的特征去动态学习邻接矩阵，对简单的图卷积层进行优化。  

ADGC 的输入为一个全局特征 \\(V_{g}\\)、K 个局部特征 \\(V_{l}\\) 以及一张预先定义的邻接矩阵为 \\(A\\) 的图。使用 \\(V_{l}\\) 和 \\(V_{g}\\) 之间的差异动态更新图中边的权重，得到 \\(A^{adp}\\)，进而通过 \\(V_{l}\\) 与 \\(A^{adp}\\) 相乘进行简单图卷积，为了稳定训练，加上残差连接：  

$$V^{out} = [FC_{1}(A^{adp} \otimes V_{l}^{in}) + FC_{2}(V_{l}^{in}), V_{g}^{in}].$$  

最终的高阶关系模块 \\(f_{R}\\) 实现为 ADGC 的层叠，因而由语义特征 \\(V^S = \\{v_{k}^S\\}\_{k=1}^{K+1}\\) 有  

$$V^{R} = \\{v_{k}^R\\}\_{k=1}^{K+1} = f_{R}(V^S).$$  

### 训练损失
分类损失 + 三元组损失。  

### 相似度衡量
余弦相似度。  

## 高阶行人拓扑学习

### 图匹配 (graph matching) 回顾
对于两张图 \\(G_{1} = (V_{1}, E_{1})\\) 和 \\(G_{2} = (V_{2}, E_{2})\\)，图匹配的目的在于在 \\(V_{1}\\) 和 \\(V_{2}\\) 之间学习一个匹配矩阵 \\(U \in [0, 1]^{K \times K}\\)，\\(U_{ia}\\) 表示 \\(v_{1i}\\) 与 \\(v_{2a}\\) 之间的匹配度 (matching degree)。  

构建一个正对称方阵 (square symmetric positive matrix) \\(M \in \mathbb{R}^{KK \times KK}\\)，\\(M_{ia;jb}\\) 表示 \\((i, j) \in E_{1}\\) 与 \\((a, b) \in E_{2}\\) 的匹配度。最优的 \\(u^{\*}\\) 可如下得到：  

$$U^{\*} = argmax_{U} U^TMU, s.t \ ||U|| = 1.$$  

![Fig 4](/images/2021/PRN28/4.png)

### Cross-Graph Embedded-Alignment Layer with Similarity Prediction
本文设计了一种跨图嵌入对齐层 (cross-graph embedded-alignment layer, CGEA) 在考虑由图匹配学习到的高阶行人拓扑信息的同时，避免敏感的一对一对齐。  

CGEA 以两张子图作为输入，输出嵌入特征，包括语义特征和行人拓扑指导下对齐的特征。  

通过层叠的 CGEA 和一个相似度预测层 \\(f_{P}\\) 实现高阶行人拓扑模块：  

$$(V_{1}^T, V_{2}^T) = F_{T}(V_{1}^R, V_{2}^R),$$
$$s_{x_{1}, x_{2}}^T = \sigma(FC_{s}(-|v_{1}^T-v_{2}^T)),$$  

其中 \\(|\cdot|\\) 表示元素绝对值。  

### 损失函数
交叉熵。  
