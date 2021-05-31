---
title: "[论文阅读笔记 -- VQA] Object-Centric Representation Learning (2021)"
date: 2021-06-01T09:48:18+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# 2104.05166 Object-Centric Representation Learning for Video Question Answering

![Fig 1](/images/2021/PRN9/1.png)

## 模型高训练度带来的问题
这类模型更倾向于捕捉浅层模式 (shallow patterns)，因而会在浅层统计量形成捷径 (creating shortcuts through surface statistics)，而非正确的系统化推理 (true systematic reasoning)。这类方法容易导致数据效率低下、深度推理能力差以及对新问题与场景的系统泛化能力有限。  

![Fig 2](/images/2021/PRN9/2.png)

## 困难的三个来源
+ 贯穿时间的目标内以及贯穿空间的目标间长距离依赖关系 (the long-range dependencies intra-objects across time and inter-objects across space)
+ 自然语言问题随意的表达方式
+ 文本语义指导下在视觉域中进行时空推断 (spatio-temporal reasoning over the visual domain as guided by the linguistic semantics)

![Fig 3](/images/2021/PRN9/3.png)

![Fig 4](/images/2021/PRN9/4.png)
