---
title: "[论文阅读笔记 -- VQA] TGIF-QA (CVPR 2017)"
date: 2021-05-27T13:49:47+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Tgif-qa: Toward spatio-temporal reasoning in visual question answering (CVPR 2017)

## 三点重要贡献
+ 提出专为视频 VQA 设计的三种新任务，需要对视频的时空推断(spatio-temporal reasoning)来正确回答问题
+ 提出一个新的大规模视频 VQA 数据集 TGIF-QA
+ 提出基于 dual-LSTM 的方法，结合注意力机制解决这一问题

## 三种新任务及传统任务
+ 对某一指定动作计数 (Repetition Count)
+ 给出某一动作的计数，探测该重复动作 (Repeating Action)
+ 识别状态转化 (State Transition)
+ Frame QA 任务：Object/Number/Color/Location

## 两类 QA 问题
+ Open-ended / fill-in-the-blank: 给出完整的或不完整的句子，系统需要猜出正确的词
+ Multiple choice: 选择题

首次在视频 VQA 任务使用时域注意力。  

## 模型输入
(v, q, a)

## 模型输出
+ OE: single word
+ MC: vector of compatibility scores

## 特征提取

### 视觉特征提取
+ ResNet-152 提取帧信息
+ C3D 提取序列信息
+ 每 4 帧采样一帧以减小帧冗余

### 文本特征提取
GloVe

## 特征编码器
+ 用两个双层 LSTM 分别作为视觉与文本编码器。  
+ 将视觉编码器的最后一个隐状态用作问题文本编码器的初始隐状态，从而将视觉信息 “carry over” 到文本编码器。  
+ 两个文本编码器之间隐状态传递类似。  

## 答案解码器
设计了三种解码器以得到回答，一种面向 MC，另外两种面向 OE。  

### MC Dec
+ 线性回归 (无 bias)，以 answer encoder 最后一个隐状态作为输入，计算得到各个候选答案的真值分数 (real-valued score)
+ 用 hinge loss 训练，max(0, 1 + sn - sp)

### OE Number Dec
+ 与 MC Dec 类似但多个 bias，得到整数值回答
+ L2 损失
+ 用于解决 repetition count 任务

### OE Word Dec
+ 线性分类器，以 question encoder 最后一个隐状态作为输入，通过得到的 confidence vector 从词典中选词
+ Softmax loss ？ (估计文中为交叉熵损失的笔误)

## 注意力机制

### Spatial Attention
+ 学习一段视频中各帧的哪些区域需要被注意
+ 得到一个 7x7 的注意力掩码，作用于 video encoder 的各个时间步
+ 需要同时结合视觉表达与文本信号，但是模型在编码完视频后才处理 QA 对，文本信息无法作为先验，通过定义另一组参数共享的双层 LSTM 解决这个问题

### Temporal Attention
+ 学习一段视频中哪些帧需要被注意
+ 在最后用于融合视觉与文本信息
