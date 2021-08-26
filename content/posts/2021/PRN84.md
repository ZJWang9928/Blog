---
title: "[论文阅读笔记 -- 频域] Focal Frequency Loss for Image Reconstruction (ICCV 2021)"
date: 2021-08-26T20:50:08+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# 2012.12821 Focal Frequency Loss for Image Reconstruction and Synthesis (ICCV 2021)

[开源代码传送门](https://github.com/EndlessSora/focal-frequency-loss)

[项目主页传送门](https://www.mmlab-ntu.com/project/ffl/index.html)

![Fig 1](/images/2021/PRN84/1.png)

## 背景

Image Reconstruction And Synthesis 任务。  

本文提出一种 Focal Frequency Loss，直接在频域中优化生成模型。  

![Fig 2](/images/2021/PRN84/2.png)

## Focal Frequency Loss

由 2D DFT 的公式表达可以看出，\\(F(u, v)\\) 是遍历图像空间域所有像素的求和函数，因而频谱中一个特定空间频率的值取决于所有图像像素，一些影响的可视化如图 2 所示。  

### Frequency Distance

F(u, v) 可以写为实部和虚部的和：  

$$F(u, v) = R(u, v) + I(u, v)i.$$  

其幅度 (amplitude) 和相位 (phase) 分别为：  

$$|F(u, v)| = \sqrt{R(u, v)^2 + I(u, v)^2},$$
$$\angle F(u, v) = arctan(\frac{I(u, v)}{R(u, v)}),$$

幅度表示能量，即一张图像对特定频率的 2D 正弦波相应有多强；相位表征 2D 正弦波的偏移。  

![Fig 3](/images/2021/PRN84/3.png)

由图 3 可视化可以看出必须同时考察幅度和相位以实现可靠的重构。  

本文对此的解决方案是将各频率值映射到为一个二维空间，即一个复平面 (plane) 中的一个欧几里德向量。  

对一个特定频率的距离定义为  

$$d(\overrightarrow{r_{r}}, \overrightarrow{r_{f}}) = ||\overrightarrow{r_{r}} - \overrightarrow{r_{f}}||^{2}\_{2} = |F_{r}(u, v) - F_{f}(u, v)|^{2},$$

从而两张图像之间的频率距离可以定义为其均值  

$$d(F_{r}, F_{f}) = \frac{1}{MN} \sum_{u=0}^{M-1} \sum_{v=0}^{N-1} |F_{r}(u, v) - F_{f}(u, v)|^{2}.$$

![Fig 4](/images/2021/PRN84/4.png)

## Dynamic Spectrum Weighting

仅凭借上一节介绍的损失函数并不够，模型依旧会偏向容易的频率。  

因而引入一个频谱权重矩阵 (spectrum weight matrix) 来降低容易频率的权重，从而使模型更专注于困难频率。  

频谱权重矩阵与频谱尺寸相同，定义为  

$$w(u, v) = |F_{r}(u, v) - F_{f}(u, v)|^{\alpha},$$ 

\\(alpha\\) 在本文实验中取 \\(1\\)，进而对矩阵中的值做归一化，梯度锁住，不参与训练。  

通过在频谱权重矩和频域距离矩阵之间做哈达玛积，得到 focal frequency loss (FFL) 的完整形式：  

$$FFL = \frac{1}{MN} \sum_{u = 0}^{M - 1} \sum_{v = 0}^{N - 1} w(u, v) |F_{r}(u, v) - F_{f}(u, v)|^{2}.$$  

\\(FFL\\) 可以视作频域距离的加权平均值。  

在实际实现时，通过 2D DFT 变换厚的频域表征首先进行了正交化处理，及除以了 \\(\sqrt{MN}\\)，以保证梯度平滑。  
