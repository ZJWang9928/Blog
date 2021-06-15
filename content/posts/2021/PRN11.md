---
title: "[论文阅读笔记 -- VQA] Hierarchical Conditional Relation Networks (CVPR 2020)"
date: 2021-06-04T13:42:56+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
<!-- draft: false -->
draft: true
---

# Hierarchical Conditional Relation Networks for Video Question Answering (CVPR 2020)

## 当前方法的问题
所构建的各个子系统都针对特定模态数据，对于数据模态、视频长度、问题类型的变化无法有效应对。  

因此本文提出一个可复用的单元结构，成为条件关系网络 (Conditional Relation Network, CRN)，将一个目标数组根据文本特征作为条件聚合并转化为一个新的数组。  
