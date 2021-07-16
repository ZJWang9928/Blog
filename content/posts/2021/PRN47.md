---
title: "[论文阅读笔记 -- 频域] High-Frequency Component Explain Generalization of CNNs (CVPR 2020)"
date: 2021-07-16T10:29:32+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "CNN"]
draft: false
---

# High-Frequency Component Helps Explain the Generalization of Convolutional Neural Networks (CVPR 2020)

![Fig 1](/images/2021/PRN47/1.png)

## 背景

从数据视角研究 CNN 的泛化表现。  

![Fig 2](/images/2021/PRN47/2.png)

## CNN 挖掘高频成分

将原始数据分解为高低频成分，\\(x = \\{x_{l}, x_{h}\\}\\)，分别简写为 LFC 与 HFC：  

$$z = \mathcal{F}(x), \quad z_{l}, z_{h} = t(z; r),$$
$$x_{l} = \mathcal{F}^{-1}(z_{l}), \quad x_{h} = \mathcal{F}^{-1}(z_{h}).$$  

其中 \\(\mathcal{F}\\) 与 \\(\mathcal{F}^{-1}\\) 分别表示傅利叶变换与逆变换，\\(t\\) 是阈值函数。  

### 阈值函数的一个例子

文中用灰度图(单通道的图)举了一个阈值函数的例子，对于一张有 \\(\mathcal{N}\\) 中可能像素取值的 \\(n \times n\\) 图像，即 \\(x \in \mathcal{N}^{n \times n}\\)，有 \\(z \in \mathcal{C}^{n \times n}\\)。用 \\(z(i, j)\\) 表示 \\(z\\) 在 \\((i, j)\\) 位置的值，用 \\(c_{i}\\) 和 \\(c_{j}\\) 表示形心 (centroid)，则 \\(z_{l}, z_{h} = t(z; r)\\) 定义如下：  

$$z_{l}(i, j) = \begin{cases} z(i, j), \qquad if \ d((i, j), (c_{i}, c_{j})) \le r \\\\ 0, \qquad \qquad otherwise.\end{cases},$$
$$z_{h}(i, j) = \begin{cases} 0, \qquad \qquad if \ d((i, j), (c_{i}, c_{j})) \le r \\\\ z(i, j), \qquad otherwise.\end{cases}.$$

这里 \\(d\\) 取欧几里德距离，若 \\(x\\) 有多个通道，则独立对各通道进行上述操作。  

### 一个结论

CNN 能够挖掘人类看不到的图像高频成分。  

即人类只可见 \\(x_{l}\\)，而 CNN 可见 \\(x_{l}\\) 与 \\(x_{h}\\)，有  

$$y := f(x; \mathcal{H}) = f(x_{l}; \mathcal{H}),$$  

但 CNN 训练如下  

$$argmin\_{\theta} \ l(f(x; \theta), y),$$  

等价于  

$$argmin\_{\theta} \ l(f(\\{x_{l}, x_{h}\\}; \theta), y),$$  

即 CNN 在最小化损失的过程中会学习挖掘 \\(x_{h}\\) 信息，因而 CNN 的泛化表现对人类而言变得不直观。  

## 鲁棒性与精度之间的权衡

### 一个结论

对于模型 \\(\theta\\)，存在一组样本\\(<x, y>\\) 使得  

$$f(x; \theta) \ne f(x_{l}; \theta).$$  

## 关于数据的重新思索

### 旨在解释如下现象

神经网络能轻易拟合标签打乱的数据 (label-shuffled data)。  

### 一个疑问

如果神经网络能够轻易记住数据，为什么要从数据中学习可泛化的模式，而不是直接记住一切来降低训练损失呢？  

### 对该问题的假设

尽管都会最小化训练损失，模型从如下两种情况下都考虑不同级别的特征：  

+ 在原始标签情况中，模型首先捕捉 LFC，进而逐渐捕捉 HFC 以获得更高的训练精度；  
+ 在标签打乱情况中，LFC 与标签的联系因打乱而消除，模型不得不在平等对待 LFC 与 HFC 的情况下记忆图像。  

![Fig 3](/images/2021/PRN47/3.png)

### 实验测试

文章接着设计了实验来测试提出的假设。  

从图 3 可以首先看出打乱后需要花更长训练时间得到相同精度，这表明了比起学习可泛化模式，直接记忆更加“不自然”。  

\\(M_{natural}\\) 倾向于学习 LFC，一种解释是神经网络倾向于更简单的函数，而因为数据集由人类标注，LFC 与标签之间的关联比 HFC 更具泛化能力，该关联在损失平面上形成最陡峭的梯度，尤其是在训练初期。  

### 幸存者偏差？

一个需要思考的问题在于，网络对 LFC 的偏好与人类认知偏好的巧合也可能只是幸存者偏差的结果。  

换言之，在神经网络的发展过程中，可能符合人类偏好的模型被保留了下来。  

对于发展中的方法究竟与人类的视觉偏好有多相合，利用频率工具进一步研究。  

## Training Heuristics

对在模型发展中对提升表现有帮助的方法进行评估，评估其对于 LFC 与 HFC 的泛化能力。  

![Fig 4/5](/images/2021/PRN47/4-5.png)

### Batch Size

小的 batch size 善于提升训练与测试精度，而更大的 batch size 在缩小泛化鸿沟上表现很好，泛化鸿沟与模型捕捉 HFC 的倾向密切相关。

### Batch Normalization

![Fig 6](/images/2021/PRN47/6.png)
