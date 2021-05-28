---
title: "[论文阅读笔记 -- VQA] Gradually Refined Attention (MM 2017)"
date: 2021-05-27T13:55:03+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Video Question Answering via Gradually Refined Attention over Appearance and Motion (MM17)

## 延伸模型的缺陷
+ 由 video captioning 与 ImageQA 等任务延伸而来的模型容易弱化或忽视视频的时域信息
+ 这些模型将整个问题编码为单一特征，不具有足够的表现能力来揭示问题所包含的全部信息，且粗粒度会遗漏关键词信息

每个 clip 包含 16 个连续的帧。  

## 特征提取
+ 从帧提取外观特征 (appearance features)，每帧相当于一张静态图像，本文用 VGG 提取特征
+ 从 clips 提取动作特征 (motion features)，本文用 C3D 提取特征
+ frame-level + clip-level 得到表征视频特征的一系列向量
+ embedding layer + LSTM 提取文本特征

![Fig 1](/images/2021/PRN2/1.png)

## Attention Memory Unit (AMU)
+ AMU 以当前时间步的单词嵌入、问题信息以及视频特征作为输入
+ 主要由四种模块构成：Attention (ATT)、Channel Fusion (CF)、Memory (LSTMa)、Refine (REF)

![Fig 2](/images/2021/PRN2/2.png)

### Attention (ATT)
+ 对于一个关于视频的问题，只有其中部分帧或 clip 与问题最为相关
+ 对 appearance 和 motion 分别提取
+ ATT1 强化当前单词的影响，ATT2 强调后续用 LSTMa 隐状态生成第二个注意力权重

### Channel Fusion (CF)
+ 对 ATT1 从 appearance 和 motion 得到的特征进行融合
+ 用当前单词嵌入求取权重

### Memory (LSTMa)
+ 用于控制 ATT2 的输入，并记住注意力历史
+ 其隐状态包含当前已处理的问题信息
+ 以 CF 得到的中间视频特征、自己的上一个隐状态以及上一个时间步的 refined video representation 作为输入

### Refine (REF)
+ ATT1 和 ATT2 得到的注意力权重的均值作为此处权重

## 答案生成 (Answer Generation)
+ 最终得到融合后的视频特征、从 AMU 的 memory 隐状态得到的注意力历史以及从 LSTMq 隐状态得到的问题特征
+ 将粗粒度的问题特征与细粒度的单词特征结合利用
+ 用 softmax 分类器解决选择问题，三个特征乘积变换
+ 用 LSTM 生成答案，用两个隐状态对 LSTM 进行初始化，用视频特征最为输入，各个单词可以用与上一项中类似方式得到

![Fig 3](/images/2021/PRN2/3.png)

## 两个 trick
+ 对于不包含在 GloVe 中的词，将所有存在的单词的词嵌入取均值作为其词嵌入
+ 注意力的可视化方式值得借鉴，对标量式注意力用条行图表征
