---
title: "[论文阅读笔记 -- 对抗样本] Generating Adversarial Examples with Adversarial Networks (IJCAI 2018)"
date: 2021-07-07T11:06:29+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "adversarial", "GAN", "adversarial samples"]
draft: false
---

# Generating Adversarial Examples with Adversarial Networks (IJCAI 2018)

## AdvGAN 模型架构

![Fig 1](/images/2021/PRN34/1.png)

$$\mathcal{L}\_{GAN} = \mathbb{E}\_{x} log \mathcal{D}(x) + \mathbb{E}\_{x} log(1 - \mathcal{D}(x + \mathcal{G}(x))),$$

$$\mathcal{L}\_{adv}^{f} = \mathbb{E}\_{x}\mathcal{l}\_f(x + \mathcal{G}(x), t),$$  

$$\mathcal{L}\_{hinge} = \mathbb{E}\_{x} max(0, ||\mathcal{G}(x)||\_{2} - c),$$

$$\mathcal{L} = \mathcal{L}\_{adv}^{f} + \alpha \mathcal{L}\_{GAN} + \beta \mathcal{L}\_{hinge}.$$  

## 样本可视化

![Fig 2](/images/2021/PRN34/2.png)

![Fig 3](/images/2021/PRN34/3.png)
