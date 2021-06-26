---
title: "[论文阅读笔记 -- 频域 / 迁移学习] FSDR: Frequency Space Domain Randomization (CVPR 2021)"
date: 2021-06-26T11:27:54+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "domain generalization"]
draft: false
---

# FSDR: Frequency Space Domain Randomization for Domain Generalization (CVPR 2021)

![Fig 1](/images/2021/PRN21/1.png)

## 背景

语义分割受限于数据标注的难度，因此带有自动生成标签的合成图像成了缓解这一问题的一个选择。  

但是这类模型由于域偏置与偏移 (domain bias and shift) 问题，其在真实图像上的表现通常急剧下降。  

无监督的域自适应学习 (unsupervised domain adaptation, UDA) 通过从一个有标签的源域和无标签的目标域学习域不变/对齐的 (domain invariant/aligned) 特征，来解决域不匹配的问题 (domain mismatch problem)。但是其训练需要目标域数据，而这在训练阶段有时很难获得。而且其必须对新的目标域重新训练或微调。  

域泛化 (domain generalization) 的训练无需目标域数据，因而广受关注。一种广泛使用的域泛化策略是域随机化 (domain randomization, DR)，通过对抗扰动 (adversarial perturbation)、GAN 等手段，对源域图像随机化或风格化，以学习域无关 (domain-agnostic) 的特征。  

但现有的 DR 方法都在空域中随机化图像的全部频谱。  

本文设计了一种频率空间域随机化 (frequency space domain randomization, FSDR) 技术，将图像变换到频率空间，通过识别并随机化随域改变的频率成分 (domain-variant frequency components, DVFs) 并保持随域不变的频率成分 (domain-invariant frequency components, DIFs) 不变，进行域泛化。  

本文探究了两种在频率空间做域随机化的方法：  
+ 基于谱分析 (spectrum analysis) 的 FSDR (FSDR-SA)，通过经验学习来识别 DIFs 和 DVFs
+ 基于谱学习 (spectrum learning) 的 FSDR (FSDR-SL) 通过动态的迭代学习过程识别 DIFs 和 DVFs

![Fig 2](/images/2021/PRN21/2.png)

## 问题定义

本文关注语义分割上的无监督域泛化 (UDK) 问题。  

![Tab 1](/images/2021/PRN21/T1.png)

## 谱分析

对于每张源域中的图像 \\(x_{s} \in \mathbb{R}^{H \times W \times 3}\\)，首先用 DCT 将其转化到频率空间，进而用一个带通滤波器 (band-pass filter) \\(B_{p}\\) 将转化后的信号分解为 64 个频率成分：  

$$f_{s} = B_{p}(D(x_{s})) = \\{f_{s}(0), f_{s}(1), \cdots, f_{s}(63)\\} \in \mathbb{R}^{H \times W \times 3 \times 64},$$

其中 \\(D\\) 表示 DCT，\\(f_{s}(i)\\) 表示频率特征。  

用过一系列控制实验来确定 \\(f_{s}\\) 中的 DIFs 和 DVFs。对各源域图像，用一个带宽拒绝过滤器 (band reject filter) \\(B_{r}\\) 过滤掉索引处于特定区间内的频率成分，用剩下的成分训练模型。将训练好的模型应用于目标域图像，表现的提升或下降意味着所去除的频率成分是否随频率不变。  

由此能够得到 \\(f = \\{f_{var}, f_{invar}\\}\\)，以及对应的二值掩码 \\(I^{SA}\\)。  

可以看出去掉高频或低频成分都能提升模型泛化能力，结合图 2 可以看出高频和低频比起中间频率成分能捕捉更多随域变化的特征。  

![Fig 3](/images/2021/PRN21/3.png)

## 域随机化中的谱分析

FSDR-SA  

与现有的使用 GAN 做图像风格迁移的 DR 方法不同，本文采用直方图匹配 (histogram matching) 进行在线的图像翻译 (online image translation)。调整源域图像的频率空间系数直方图，使其与目标图像的相似，这一过程通过匹配其累积密度函数 (cumulative density function) 实现。  

## 基于 FSDR 的谱学习

FSDR-SL  

通过迭代学习识别 DIFs 和 DVFs，用交叉熵实现 FSDR-SL。  

交叉熵用于衡量类重叠 (class overlap)，即预测到的交叉熵随着类重叠的增加而减小。利用这一性质， FSDR-SL 根据目标域图像的交叉熵识别 DIFs 和 DVFs。  

FSDR-SL 是用源域图像分解后的多通道频率成分训练的。如果训练后的模型对一张真实目标图像产生交叉熵小(置信度高)的预测，则说明目标图像被激活的频率成分具有较好的跨域语义不变性，在这种情况下，这些频率成分被识别为 DIFs，随机化会作用于其他频率成分。 

![Fig 4](/images/2021/PRN21/4.png)
