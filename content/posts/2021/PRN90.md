---
title: "[论文阅读笔记 -- 跨模态检索] Unsupervised Cross-modal Retrieval through AL (ICME 2017)"
date: 2021-09-07T14:45:21+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention", "unsupervised"]
draft: false
---

# Unsupervised Cross-modal Retrieval through Adversarial Learning (ICME 2017)

![Fig 1](/images/2021/PRN90/1.png)

## 概述

本文提出基于对抗学习的无监督跨模态检索 (Unsupervised Cross-modal Retrieval with Adversarial, UCAL)。  

包含四个部分：  

+ 图像特征映射
+ 文本特征映射
+ 模态分类器，生成二元特征，用二元交叉熵做损失函数
+ 特征相关性 (\\(\mathcal{l}_{2}\\) 范数)
