---
title: "[论文阅读笔记 -- VQA] Motion-Appearance Co-Memory Networks (CVPR 2018)"
date: 2021-05-28T14:06:27+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Motion-Appearance Co-Memory Networks for Video Question Answering (CVPR 2018)

## 视频 QA 与 图像 QA 相比三个独有特性
+ 处理较长的图像序列，包含更丰富的信息（数量上及多样性上）
+ 动作与外观信息通常互相关联，能够彼此提供有用的注意力线索
+ 不同问题需要不同数量的帧来推断回答

## 注意力机制 + 记忆力机制
+ 注意力机制告诉网络“看哪里”
+ 记忆力机制在多步循环中精炼答案

## Dynamic Memory Networks (DMN) 缺陷
+ 缺少动作分析 (motion analysis)，有其实对视频中动作与外观的联合分析
+ 缺少时域建模
+ 本文基于 DMN/DMN+，根据 VQA 独有特性提出 motion apprearance co-memory network

## General DMNs
DMN 由四个模块构成。

### Fact Module
+ 将输入数据转化为一组被成为 facts 的向量，用 F 表示
+ 用 GRU 编码 text-based QA 中的文本信息，用 bi-GRU 编码 image-based QA 中的局部视觉区域特征

### Question Module
+ 将问题转化为嵌入 q
+ 通常是 GRU 的最后一个隐状态作为问题嵌入

### Episodic Memory Module
+ 用于从 facts 中检索相关信息，对输入的 facts 进行多轮循环迭代，更新记忆力单元
+ 包含两个重要机制：注意力机制 + 记忆力更新机制
+ 为了有效使用视频的顺序与位置信息，设计了基于注意力的 GRU，用注意力门取代 GRU 原本的更新门
+ 注意力门用第 i 步的 fact 向量、第 t-1 轮循环的记忆力单元以及问题嵌入作为输入，输出一个标量作为第 i 个 fact 在 循环 t 中的注意力值
+ 注意力 GRU 最后一个隐状态被用作上下文特征，与问题嵌入、 t-1 循环的注意力一同用于更新 episodic memory
+ 最终的 memory 被传入 answer module 用于生成最终的答案

### Answer Module
+ 问题嵌入 + 最终 memory 用于答案生成

## Motion-Appearance Co-Memory Networks

### 多级上下文 facts (Multi-level Contextual Facts)
+ 视频被分割成小单元，对各个单元用两支 CNN 提取单元级别的(unit-level)动作与外观特征
+ 利用时域卷积 + 反卷积，构建多级的时域特征表达，其中各级特征蕴含不同的上下文信息
+ 最终上采样得到时域粒度更粗但是语义更强的特征序列，与下采样前特征分辨率相同，但时域上下文覆盖范围不同
+ 最终得到多级的 contextual facts

### Co-memory Attention
+ 使得外观与动作间信息得以交互
+ 两个独立的注意力模块分别建模外观与动作注意力
+ t-1 步的两种注意力对 t 步的两种注意力更新都共同作用
+ 得到两个注意力门

### Dynamic Fact Ensemble

#### 需要动态选择 facts 的两个原因
+ 不同问题需要不同层级的特征
+ 在多重循环过程中，各循环所关注的信息层级可能不同

#### 基于注意力的 fact 集成方法
+ 对上一节得到的门沿着层级维度(i)计算 Softmax 得到注意力分数，用于对 fact 进行加权和  
+ 后续的 fact 编码阶段用到的注意力分数为门沿着层级维度求均值后取 Softmax

### 记忆力更新
+ A-GRU 分别生成外观和动作的上下文特征向量
+ 对外观和动作的 memory 分别更新，FC + ReLU，输入为前一步 memory、问题嵌入 q以及上下文特征
+ 最后拼接两个 memory

## 回答生成
与前几篇文章相似  
