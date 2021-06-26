---
title: "[论文阅读笔记 -- 对抗样本] AI-GAN: Attack-Inspired Generation of Adversarial Examples (2020)"
date: 2021-06-26T15:10:01+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid", "hashing"]
draft: false
---

# 2002.02196 AI-GAN: Attack-Inspired Generation of Adversarial Examples (2020)

本文设计了一种新的 GAN 的变体，称为 Attack-Inspired GAN (AI-GAN) 用于生成对抗扰动。  

AI-GAN 的训练包含两个阶段：  
+ 第一阶段，联合训练一个生成器、一个鉴别器和一个攻击器 (attacker)。攻击器用于激励生成器在联合训练过程中快速产生有效扰动，鉴别器评估分别由生成器和攻击器生成的对抗样本之间的相似度。
+ 第二阶段，移除攻击器，使用标准的 GAN 训练策略。由于此时鉴别器评估的是生成器产生的图像与原图之间的相似度，对生成器的输出进行微调 (fine-tune)。

## 任务目的

生成一个对抗样本 \\(x_{a}\\)，使其在某些距离度量下与原始样本 \\(x\\) “看起来相似”，但是能让模型输出一个错误的结果。  

攻击分为如下两类：  

### 无目标攻击 (Untargeted Attack)
对于一组实例 \\((x, y)\\)，对抗样本使得 \\(f(x_{a} \ne y)\\)。  

### 有目标攻击 (Targeted Attack)
对于一组实例 \\((x, y)\\)，对抗样本使得 \\(f(x_{a} = t)\\)，\\(t\\) 为一个目标类别。  

有目标攻击比无目标攻击更具挑战性，本文主要关注有目标攻击。  

![Fig 1](/images/2021/PRN22/1.png)

## 模型架构

AI-GAN 包含一个生成器 \\(G\\)、一个鉴别器 \\(D\\)、一个攻击器 \\(A\\) 以及一个目标分类器 (target classifier) \\(C\\)。  

![Fig 2](/images/2021/PRN22/2.png)

### AI-GAN 与 AdvGAN 架构上两大主要差异

+ AdvGAN 在训练时固定目标类，这制约了生成器只能产生以单个类别为目标的对抗扰动，要为多个类别生成扰动则需要训练多个模型；而 AI-GAN 以原图和目标类别作为输入，能够为产生不同的目标类别产生对抗扰动。
+ 像 AdvGAN 那样简单地将原始图像嵌入为目标类别较难训练，因而 AI-GAN 采用了两个阶段来训练，以解决这一问题。  

AI-GAN 中，生成器 \\(G\\) 以一张干净的图像 \\(x\\) 以及随机采样得到的目标类别标签 \\(t\\)最为输入，生成对抗扰动 \\(G(x, t)\\)，进而得到对抗样本 \\(x + G(x, t)\\) 传入鉴别器 \\(D\\)。

### 第一阶段

\\(D\\) 用于区分两类对抗样本 \\(x + G(x, t)\\) 和 \\(A(x, t)\\)，\\(G\\) 和 \\(A\\) 所使用的目标类别相同。  

### 第二阶段

用原始图像样本替代 \\(A\\) 生成的样本，使用标准的 GAN 训练方式。

两个阶段中，目标分类器 \\(C\\) 都以 \\(x + G(x, t)\\) 作为输入，输出一个相应的几率 (logits)，用于计算一个损失 \\(L_{adv}^{C}\\)，其表示所生成对抗样本的有效性。  

### 损失函数

模型的整体损失函数为：  

$$L = L_{adv}^{C} + \lambda L_{GAN} + \gamma L_{hinge},$$  

其中，\\(L_{adv}^{C}\\) 是用于欺骗目标分类器 \\(C\\) 的损失，表示为：  

$$L_{adv}^{C} = \mathbb{E}_{x} l_{C}(x + G(x), t),$$  

\\(l_{C}\\) 表示用于训练目标分类器 \\(C\\) 的损失函数。  

\\(L_{GAN}\\) 是对抗损失，用于 GAN 的训练。第一阶段为：  

$$L_{GAN}^{1} = \mathbb{E}_{x} log D(A(x, t)) + \mathbb{E}_{x} log(1 - D(x + G(x, t))),$$

第二阶段为：  

$$L_{GAN}^{2} = \mathbb{E}_{x} log D(x) + \mathbb{E}_{x} log(1 - D(x + G(x, t))),$$

\\(L_{hinge}\\) 是合页损失 (hinge loss)，用于对扰动的大小进行限制：  

$$L_{hinge} = \mathbb{E}_{x} max(0, ||G(x)||_{2} - c),$$  

其中 \\(c\\) 表示一个人为定义的常量。  

##  攻击成功率对比

![Fig 3](/images/2021/PRN22/3.png)

## 对抗样本可视化

![Fig 4](/images/2021/PRN22/4.png)

到了第二阶段，AI-GAN 生成的扰动比第一阶段肉眼可见地小了，而 AdvGAN 变化不明显。  
