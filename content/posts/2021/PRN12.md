---
title: "[论文阅读笔记 -- 跨模态检索] Deep Adversarial Graph Attention Convolution Network (MM 2019)"
date: 2021-06-15T20:35:36+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid"]
draft: false
---

# Deep Adversarial Graph Attention Convolution Network for Text-Based Person Search (MM 2019)

## 先前工作的问题
+ 孤立对待图像中的局部块，只考虑文本描述中单词级别的上下文关联，因而忽略了图文所包含的结构化语义信息 (structured semantic information)
+ 学习公共空间的方法通常使用双向 ranking loss 以及 canonical correlation analysis (CCA) 等损失函数，这些损失函数收敛慢且不稳定。此外，这些方法只考虑配对的图文信息，忽略了各模态内部的整体数据分布，而这能被用于在模态之间形成桥梁  

本文提出 deep adversarial graph attention convolution network (A-GANet) 学习 modality-invariant 的图文特征。  

![Fig 1](/images/2021/PRN12/1.png)

## 图像图注意力网络 (Image Graph Attention Network)
基于 ResNet-50，由 5 个残差块、一个视觉场景图模块 (visual scene graph module) 和一个联合嵌入层 (jonit embedding layer) 构成，用于从图像数据提取视觉特征。  

5 个残差块输出的特征图经平均池化得到 2048 维的全局外观视觉特征 (global appearance visual feature)。  

通过视觉场景图模块构建视觉场景图 \\(G_{v} = (O_{v}, E_{v})\\)，其中 \\(O_{v} = \\{o_{i}\\}_{i=1}^N\\) 表示由各目标构成的节点集合， \\(E _{v} = \\{e _{ij}\\}\\)表示节点 \\(o _i\\) 与 \\(o _{j}\\) 之间关联性构成的有向边集合， \\(e _{ij}\\) 由三元组 \\(<subject, predicate, object>\\) 表示。  

用 Faster-RCNN + ResNet-50 作为目标检测器， Motifs relationship classifier 用于预测目标之间的关系。此外，用一个设计好的属性分类器 (designed attribute classifier) 预测所得到目标的属性。  

提取四种特征：外观特征 \\(v _{a}\\)、空间特征 \\(v _{s}\\)、标签嵌入 \\(v _{l}\\) 以及属性嵌入 \\(v _{att}\\)，拼接构成节点特征 \\(v _{o _{i}}\\)。  

\\(v _{e _{ij}}\\) 的构建，从相应的 subject 区域和 object 的并集提取前三种特征拼接得到。  

为了从有向图中提取结构化的语义视觉特征，设计了图注意力卷积 (graph attention convolution, GAConv) 层。

$$v _{o _{i}} = \sum _{o _{j} \in sbj(o _{i})} w _{ij} \cdot g _{s}(v _{o _{i}}, v _{e _{ij}}, v _{o _{j}}) + \sum _{o _{k} \in obj(o _{i})} w _{ik} \cdot g _{o}(v _{o _{k}}, v _{e _{ki}}, v _{o _{i}}),$$

其中 \\(g _{s}\\) 和 \\(g _{o}\\) 表示 FC + BN + ReLU， \\(w _{ij}\\) 表示节点 j 到节点 i 的重要性，

## 文本图注意力网络 (Text Graph Attention Network)

![Fig 2](/images/2021/PRN12/2.png)
