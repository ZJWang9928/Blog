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

## 文本问题编码
GloVe + Bi-LSTM  

\\(e_{s} = \[\overleftarrow{h_{s}}, \overrightarrow{h_{s}}\]\\) 得到第 s 个单词的上下文单词特征 (contextual word representation)，\\(q_{g} = \[\overleftarrow{h_{1}}\, \overrightarrow{h_{S}}]\\) 得到全局序列化特征 (global sequantial representation)，S 为句子长度。将二者融合得到最终的问题特征：  

\\[q = \sum_{s=1}^{S}e_{s} \cdot softmax_{s}(w_{q}^T(e_{s} \odot q_{g})).\\]

![Fig 4](/images/2021/PRN9/4.png)

## 目标为中心的视频特征 (Object-centric Video Representation)
基于目标在时空中的 3D 结构 (称为 tubelet) 表示一个目标。各个时间步中一个目标为一个由目标跟踪 (object tracking) 赋予唯一编号的 2D 选框，通过将视频中具有同样编号的 tubelet 联接起来得到 tubelet。  

若将一段视频看作事件的集合，那么目标之间的交互通常发生在一段较短的时间内。故此，将 object tubelets 划分为 sub-tubelets，每个 sub-tubelet 等同于一个时域部件。  

### 构建 Object Tubelets
#### 目标检测与跟踪

用 Faster R-CNN 对每帧进行目标检测，得到 2048 维的外观特征 \\(v_{a}\\) (表示“是什么”)、选框 \\(v_{b} = \[x_{min}, y_{min}, x_{max}, y_{max}\]\\) 以及确信分数 (confidence score)。  

使用 DeepSort 进行目标跟踪，其利用检测到选框的外观特征及确信分数进行跟踪，最同一目标的选框标注唯一的编号。为了方便实现，假设每个目标的生命周期都覆盖整段视频，某一时刻缺失的目标用空值 (null value) 标记。  

最终各个目标都表示为一个 tubelet，由一个唯一 ID、一系列选框坐标以及一组相应的外观特征构成。  

#### 对“是什么”和“在哪里”进行联合编码
为了表征某时刻各个 tubelet 的集合位置信息，引入一个 7 维的空间向量 \\(v_{p} = \[\frac{x_{min}}{W}, \frac{y_{min}}{H}, \frac{x_{max}}{W}, \frac{y_{max}}{H}, \frac{\Delta x}{W}, \frac{\Delta y}{H}, \frac{\Delta x \Delta y}{WH}\],\\) 分别表示相对于整个视频帧面积的坐标、宽、高以及面积。  

对于一个动作发生的某一较短时刻，一个目标的外观特征\\(v_{a}\\) 可能变化极小，而其位置的改变则可能更为明显。如一个人走向一辆车的过程。故此，使用一个乘法门控机制 (multiolicative gating mechanism) 来编码目标的特定位置外观 (position-specific appearance):  

\\[v_{ap} = f_{1}(v_{a}) \odot g_{a}(v_{p}),\\]

其中 \\(f_{1}(v_{a})\\) 是非线性映射函数， \\(g_{1}(v_{p}) \in (0, 1)\\) 是位置门控函数，本文具体实现为 tanh 和 sigmoid。  

此外，为了弥补不完美的目标检测算法带来的损失，引入 2048 维的上下文目标 (contextual object) \\(v_{c}\\)，有预训练的 ResNet 对每帧提取得到。  

### 时域部件基于语言条件的特征 (Language-conditioned Representation of Temporal Parts)
得到目标的特定位置外观特征后，通过将若干帧作为一个 clip 降低复杂度，通过考虑问题特征构建一个基于语言条件的时域部件。其目的是要使得各部件 question-specific，为后续的相关性推理做准备。  

将一个目标的生命周期 K 等分成时域部件集合 C，每个 \\(C_{k}\\) 包含 t 帧，即 t 个 \\(v_{ap}\\)。对于给出的句子特征 q，引入一个时域注意力机制处理 \\(C_{k}\\)：  

\\[\alpha_{k, i} = softmax_{i}(w_{s}^T((W_{q}q+b_{q}) \odot (W_{v}v_{ap}^{k,i} + b_{v}))),\\]

\\[c_{k} = \lambda \cdot \sum_{i=1}^{t}\alpha_{k, i}v_{ap}^{k, i},\\]

其中 \\(\lambda\\) 是二值掩码向量，用于排除标记为空值的缺失目标。  

### 基于查询条件的目标图 (Query-conditioned Object Graph)

处于同一时段的时域目标部件具有高度关联性，这取决与目标和 query 语义之间的空间距离。用一张图 \\(G_{k} = (C_{k}, A_{k})\\) 表征时域部件 k 中的目标及其相互之间的关系。其中 \\(C_{k} =\\{c_{k, o}\\}_{o=1}^N\\) 表示节点，而邻接矩阵 \\(A_{k} \in \mathbb{R}^{N \times N}\\) 与 query 有关：  

\\[a_{k,o} = softmax_{o}(w_{a}^T(\[c_{k,o}, c_{k,o} \odot q\])),\\]

\\[A_{k} = a_{k}^T a_{k},\\]

其中 \\(c_{k,o}\\) 是目标 o 中部件 k 的特征。  

进而可以用 GCN 进行运算，将 \\(v_{c}\\) 作为额外节点，构建异构图 (heterogeneous graph)，目标之间双向，上下文节点到其他节点单向，两层 GCN 之间加入跳连，最终得到 \\(\tilde{c_{k}}\\)。  

### Video as Evolving Object Graphs
用 Bi-LSTM 处理 \\(c_{k}\\)，最终得到各个目标的 resume \\(r = \[\overleftarrow{h_{1}}, \overrightarrow{h_{K}}\]\\)，共 N 个。

## 关联性推理
至此，得到了两个集合，上下文视觉简历 \\(R = \\{r_{o}\\}_{o=1}^N\\) 和查询特征 \\(Q = \\{q_{g}, e_{1}, e_{2}, ..., e_{S}\\}\\)。  

将上述集合作为知识库，动态构建语言指导下对目标关系的预测，并将这一系列预测按正确顺序串起来以得到答案。可以使用 MACNet、LOGNet等生成式推理引擎。  

## 答案解码器
与前文类系。  
