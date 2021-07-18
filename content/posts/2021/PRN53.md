---
title: "[论文阅读笔记 -- 跨模态检索 / 图网络] Cross-Graph Attention Model (SIGIR 2021)"
date: 2021-07-18T11:25:44+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "attention", "cross-modal", "retrieval", "graph", "GCN"]
draft: false
---

# Cross-Graph Attention Enhanced Multi-Modal Correlation Learning for Fine-Grained Image-Text Retrieval (SIGIR 2021)

![Fig 1](/images/2021/PRN53/1.png)

## 背景

SIGIR 2021 短文。  

### 现有三类跨模态检索方法
+ 全局相关性学习
+ 局部相关性学习
+ 高阶语义概念学习

在模态特定的语义概念 (modality-specific semantic concepts) 之外，还应当考虑共享的语义概念 (shared semantic concepts)。  

本文提出一种跨图注意力模型 (Cross-Graph Attention Model, CGAM)，为图文模态均构建全连接带权图，通过图注意力网络更新节点。  

## 特征编码

### 视觉表征

bottom-up attention model + 全连接层。  

表征为 \\(H^{\mathcal{I}} = \\{h_{1}^{\mathcal{I}}, \cdots, h_{n_{o}}^{\mathcal{I}}\\}\\)，目标的类别与属性表征为 \\(O = \\{o_{1}, \cdots, o_{n_{o}}\\}\\)。  

### 文本表征

Bi-GRU。  

表征为 \\(H^{\mathcal{T}} = \\{h_{1}^{\mathcal{T}}, \cdots, h_{n_{w}}^{\mathcal{T}}\\}\\)。  

## 跨图注意力模型

平滑图文之间的语义差异，同时强化共享的语义概念以提纯特征表征，首先如下拼接图文特征：  

$$Z = \left[ \begin{matrix} H^{\mathcal{I}} \\\\ H^{\mathcal{T}} \end{matrix} \right].$$  

通过四张全连接带权图构建模态内与模态间的语义关联：\\(G_{I \rightarrow I} = (V_{1}, E_{1})\\)，\\(G_{I \rightarrow T} = (V_{2}, E_{2})\\)，\\(G_{T \rightarrow I} = (V_{3}, E_{3})\\)，\\(G_{T \rightarrow T} = (V_{4}, E_{4})\\)。  

邻接矩阵构建如下：  

$$Q_{i} = FC_{q}(H^{\mathcal{I}}), K_{i} = FC_{k}(H^{\mathcal{I}}), Q_{t} = FC_{q}(H^{\mathcal{T}}), Q_{t} = FC_{k}(H^{\mathcal{T}}),$$
$$A_{I \rightarrow I} = Q_{i}K_{i}^T, A_{I \rightarrow T} = Q_{i}K_{t}^T, A_{T \rightarrow I} = Q_{t}K_{i}^T, A_{T \rightarrow T} = Q_{t}K_{t}^T,$$
$$ A = \left[ \begin{matrix} A_{I \rightarrow I}/N & A_{I \rightarrow T}/N \\\\ A_{T \rightarrow I}/N & A_{T \rightarrow T}/N \end{matrix} \right], \quad N = n_{o} + n_{w}.$$

共享语义概念特征 \\(C\\\) 计算如下：  

$$C = FC(AZW + Z).$$  

## 特征重构

用共享概念特征重构模态特定的特征。  

$$W^{\mathcal{I}} = softmax(H^{\mathcal{I}}C^T), \quad W^{\mathcal{T}} = softmax(H^{\mathcal{T}}C^T),$$
$$R^{\mathcal{I}} = W^{\mathcal{I}}C, \quad R^{\mathcal{T}} = W^{\mathcal{T}}C.$$  

## 多头注意力的计算

将 \\(H\\) 和 \\(R\\) 均划分成 \\(K\\) 组，分别计算余弦相似度，拼接成向量用两层夹 tanh 的 FC 求得相似度。  

## 损失函数

三元组损失。  
