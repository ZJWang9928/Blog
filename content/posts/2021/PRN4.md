---
title: "[论文阅读笔记 -- VQA] Structured Two-stream Attention Network (AAAI 2019)"
date: 2021-05-28T15:21:20+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "vqa"]
draft: false
---

# Structured two-stream attention network for video question answering (AAAI 2019)

## 图像 QA 中两种注意力机制
+ visual attention: "where to look"
+ question attention: "what words to listen to"

## 视频 QA 三个主要困难
+ 考虑长距离时域结构，同时不遗漏重要信息
+ 为了定位相关视频实例，视频背景的影响需要被最小化
+ 视频段(segmented)信息与文本信息需要被较好融合

## 一个发现
在回答某些 VQA 问题时，需要关注多个帧，且这些帧同等重要。  

基于这一发现，本文设计了一种新的结构，命名为 structured segment，结构化段，将视频特征分为 N 个段，并将各个段作为一个共享的注意力模型的输入，从而能够从多个段中得到许多重要的帧。  

本文提出一个 Structured Two-stream Attention Network (STA)，协同利用视频的空间与长距离时域信息以及文本信息。  

![Fig 1](/images/2021/PRN4/1.png)

## 结构化段

### 视频特征提取
+ 用 ResNet-152 提取视频帧的外观特征 (2048-D)，表征该帧的全局信息
+ 用 LSTM 编码序列化数据流，t 时刻用第 t 帧特征及 t-1 时刻隐状态作为输入

### 结构化段
+ 如 [TGIF-QA](http://jonathanwayy.xyz/2021/prn1/) 等方法中直接用双层 LSTM 编码视频信息可能会遗漏关键帧信息
+ 本文首次使用单层 LSTM 得到 T 个隐状态，T 为帧数，接着将其分为 N 个段，及 N 个包含 K 个帧特征的集合，用于表示视频特征

## 文本编码器
+ GloVe + 单层 LSTM
+ MC 拼接答案；OE 只处理问题

## 结构化双分支注意力模块 (Structured Two-stream Attention Module)
用于连结并融合视频段与文本信息，共有 N 个双分支注意力块，注意力块之间共享参数。  

第 i 个块以第 i 个视频段编码特征\\(VE_{_i}\\)和文本特征\\(E\\)作为输入，学习二者之间的交互性，并对二者进行更新。  

与诸如 [TIGF-QA](http://jonathanwayy.xyz/2021/prn1/) 及 [Co-memory](http://jonathanwayy.xyz/2021/prn3/)等方法直接拼接视频帧特征与文本问题特征以得到新的特征用于答案预测不同，本文按两个方向计算注意力：视频到问题、问题到视频。  

注意力分数都由一个共享的关联性矩阵 \\(A_{i} \in \mathbb{R}^{K \times M}\\) 计算得到 \\[A_{i} = (VE_{i})^{T}W_{s}E,\\] 其中\\(W_{s}\\)是一个可学习的权重矩阵，\\(M\\) 为单词数。为了计算方便，用两个独立的线性投影取代上式 \\[A_{i} = (W_{v}VE_{i})^{T}(W_{q}E),\\] 其中 \\(W_{v}\\) 和 \\(W_{q}\\) 是线性函数的参数。  

实际上 \\(A_{i}\\) 编码的就是两个输入之间的相似度（内积），由 \\(A_{i}\\) 可计算注意力并沿着两个方向见注意力用于双分支特征。  

### 第一分支：视觉注意力
视觉注意力向量表征的是“视频中哪些帧需要关注，或与问题中各个单词最具关联性”，按如下公式计算得到 \\[C_{i} = softmax(max_{col}(A_{i})^{T}),\\] 其中 \\(C_{i} \in \mathbb{R}^{K \times 1}\\)，\\(max_{col}\\) 表示沿着列求最大值。进而处理后的视频特征为 \\[\tilde{V_{i}} = \sum_{k=1}^{K} c_{ik}ve_{ik},\\] 其中 \\(\tilde{V_{i}} \in \mathbb{1 \times D}\\) 为根据整个问题信息处理过后的视频特征向量。

### 第二分支：文本注意力
文本注意力向量表征的是“问题中那个单词需要关注”，通过对 \\(A_{i}\\) 按行求 softmax 得到 \\[B_{i} = softmax(A_{i})\\], 其中 \\(B_{i} \in \mathbb{R}^{K \times M}\\)，用其处理问题特征 \\[\tilde{E_{i}} = B_{i}E^{T},\\] 其中 \\(\tilde{E_{i}} \in \mathbb{K \times D}\\)，按列求和得到最终的 \\(\tilde{E_{i}} \in \mathbb{K \times D}\\) (文中此处符号使用略有不妥)。

## 结构化双分支融合 (Structured Two-stream Fusion)
计算完各段的特征后，首先各自求和 \\[\tilde{V} = \sum_{i=1}^{N} \tilde{V_{i}},\\] \\[\tilde{E} = \sum_{i=1}^{N} \tilde{E_{i}}.\\]  

Sum pooling 之后，按下式求得 H 用于答案预测 \\[H = ReLU(W_{fv}\tilde{V} + b_{v}) \otimes ReLU(W_{fq}\tilde{E} + b_{q}).\\]

## 答案解码器
依旧与前文类似。  
