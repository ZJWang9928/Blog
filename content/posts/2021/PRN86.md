---
title: "[论文阅读笔记 -- ReID] Learn 3D Shape Feature for Texture-insensitive Person ReID (CVPR 2021)"
date: 2021-08-28T17:30:30+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "3D", "reid", "retrieval"]
draft: false
---

# Learning 3D Shape Feature for Texture-insensitive Person Re-identification (CVPR 2021)

[开源代码传送门](https://github.com/TencentYoutuResearch/PersonReID-YouReID)

![Fig 1](/images/2021/PRN86/1.png)

## 背景

Person ReID 任务。  

研究表明 Person ReID 相当依赖衣着外观纹理 (clothing appearance textures)，大多数现有方法在衣着纹理较迷惑时表现大幅下降。由于人们可能会换衣服，且不同的人可能会穿相似的衣服，因而衣着纹理对 ReID 而言并不可靠。  

本文对更具有鉴别能力的特征进行建模，即人类形状表征 (human shape representation)。  

### 现有方法学习形状相关特征的两种方法

+ 2D 图像空间。基于 2D 的方法主要尝试基于如等高线、关键点等视觉统计量来提取形状特征，或通过 adversarial feature disentangle，这些方法只利用了 2D 空间中的结构和形状信息，而深度以及 3D 相对位置等 3D 信息被忽略。
+ 3D 源数据。基于 3D 的源数据需要通过外部设备来获取，在监控摄像环境中可能难以实现。

本文通过将 3D 重构作为 ReID 特征学习的辅助任务与正则化，直接从原始的 2D 图像中提取对纹理不敏感的 3D 特征。采用一种多任务框架 (multi-task framework)，ReID 特征受到识别 (identification) 损失 (即 softmax loss + triplet loss)，以及 3D 重构损失的监督。  

训练 3D 行人重构的一大障碍在于 3D ground truth 的缺失，因而本文设计了一种纯无监督的架构，成文对抗自监督投影 (Adversarial Self-Supervised Projection, ASSP)。
