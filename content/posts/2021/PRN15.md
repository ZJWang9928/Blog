---
title: "[论文阅读笔记 -- 跨模态哈希] Self-Supervised Adversarial Hashing Networks (CVPR 2018)"
date: 2021-06-22T10:54:41+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid", "hashing"]
draft: false
---

# Self-Supervised Adversarial Hashing Networks for Cross-Modal Retrieval (CVPR 2018)

## 当前(当时)跨模态哈希方法的主要不足
+ 直接使用单类标签来衡量跨模态的语义关联，而事实上标准的跨模态数据集中一个图像实例往往能对应多个类别标签，从而使得语义关联能被描述的更加精确
+ 编码长度通常不超过 128 位，这意味着大部分有用信息都变得无效，相反高维的模态特征包含更丰富的信息，能弥合模态差异

因而本文提出一种自监督的对抗哈希 (self-supervised adversarial hashing, SSAH) 方法。  

![Fig 1](/images/2021/PRN15/1.png)

对于两串长度为 \\(K\\) 的编码，其汉明距离可由其内积求得：  

$$dis_{H}(b_{i}, b_{j}) = \frac{1}{2}(K - <b_{i}, b_{j}>).$$  

由上式也可以看出，内积越大的两串编码越相似，从而在汉明空间量化编码相似度的问题转化为了计算编码原始特征 (codes' original features) 之间内积的问题。  

## 自监督语义生成
使用多标签标注 (multi-label annotation) 作为从更加细粒度级别联结模态之间语义关系的辅助。  

设计一种端到端的全连接网络，称为 LabNet，以对不同模态间的语义关联进行建模。  

LabNet 从一个实例的多标签向量 (one-hot 形式) 提取抽象语义信息，用于监督 ImgNet 和 TxtNet 的特征学习过程。  

## 特征学习
在 LabNet 监督下训练 ImgNet 和 TxtNet。  

## 对抗学习

构建两个鉴别器，分别判断标签特征与视觉/文本编码。  
