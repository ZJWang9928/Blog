---
title: "[论文阅读笔记 -- 跨模态检索 / 图网络] Fine-grained Video-Text Retrieval with HGR (CVPR 2020)"
date: 2021-07-26T11:53:00+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "graph", "GCN"]
draft: false
---

# Fine-grained Video-Text Retrieval with Hierarchical Graph Reasoning (CVPR 2020)

[开源代码传送门](https://github.com/cshizhe/hgr_v2t)

![Fig 1](/images/2021/PRN59/1.png)

## 核心

提出一种层级式的图推理模型 (Hierarchical Graph Reasoning model, HGR)，将视频-文本匹配分解为三级语义：  

+ 全局事件，在文本中对应整个句子
+ 局部动作，在文本中对应动词
+ 实体 (entities)，在文本中对应名词短语

![Fig 2](/images/2021/PRN59/2.png)

## 层级化文本编码

### 语义角色图结构 (Semantic Role Graph Structure)

## 层级化视频编码

## 视频-文本匹配
