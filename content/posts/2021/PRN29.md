---
title: "[论文阅读笔记 -- 对抗样本 / ReID] Vulnerability of Person Re-Identification Models (CVPR 2020)"
date: 2021-07-02T11:05:32+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "adversarial", "GAN", "adversarial samples"]
draft: false
---

# Vulnerability of Person Re-Identification Models to Metric Adversarial Attacks (CVPR 2020)

## 背景

闭集 (closed-set) 任务，即训练与测试使用同样类别的任务上对抗样本已经有了较广泛的研究，开集 (open-set) 任务如 ReID 上的相关研究较少。  

为了骗过 ReID 模型，需要扰动图像以改变其特征之间的距离，称为度量攻击 (metric attacks)。度量攻击依赖一个称为指导 (guide) 的参照特征 (reference feature)，使被攻击的图像与其他相似图像之间的距离失真。  

指导可分为两种：  

+ 拉动指导 (pulling guide)：属于另一行人 ID，将特征从其原始的 ID 簇拉远
+ 推动指导 (pushing guide)：属于同一行人 ID，将特征从其原始的 ID 簇推开

实际应用中指导类别的选择取决于各种指导是否可获取。  

## 度量攻击 (Metric Attacks)

在 ReID 任务中，对抗攻击无法将模型误导到一个错误类别上，因为类别在开集任务测试时不可知，需要扰动图像以使特征之间距离失真。为了实现这一目的，度量攻击需要一个参照特征作为指导，指导会引入上述的两种扰动。  

此时就产生一个问题 —— 两种指导的效果是否一样？  

拉动指导在一些情况中作用有限。  

对于推动指导，根据三角不等式有  

$$D(f_{\hat{x}\_{i}}, f_{g}) \le D(f_{\hat{x}\_{i}}, f_{x_{j}}) + D(f_{x_{j}}, f_{g}),$$  

\\(x_{i}\\) 是原图像，\\(\hat_{x}\_{i}\\) 是攻击后的图像，\\(x_{j}\\) 和 \\(g\\) 是两张和 \\(x_{i}\\) ID 相同的图像。推动指导增加 \\(D(f_{\hat{x}\_{i}}, f_{g})\\)，假设 \\(D(f_{x_{j}}, f_{g})\\) 较小 (\\(\mathcal{l}(x_{j}) = \mathcal{l}(g)\\) 时)，则 \\(D(f_{\hat{x}\_{i}}\\) 增加。  

这意味着在嵌入空间中，通过增大对抗图像与指导之间的距离，对抗图像同样会远离其他与指导相似的图像。但是对抗特征可能会被推到一个没有其他 ID 特征的方向(区域)。  

此外，指导的使用意味着需要能够获取额外的图像，在实际中是否能够满足也是个问题。  

![Fig 1](/images/2021/PRN29/1.png)

## 自度量攻击 (Self Metric Attack, SMA)

当无法获取额外图像，即 \\(A = \phi\\) 时，通过 SMA 使用图像自身作为推动指导，即将扰动后图像的特征与其原特征推开，用 \\(f_{x_{i}}\\) 取代了 \\(f_{g}\\)。  

$$\hat{x}^{(0)} = x + \eta,$$
$$\hat{x}^{(n+1)} = \Pi_{x}^{\epsilon}(\hat{x}^{(n)} + \alpha \cdot sign(\Delta_{n}^{SMA})),$$
$$\Delta_{n}^{SMA} = \frac{\partial D(f_{\hat{x}^{(n)}}, f_{x})}{\partial \hat{x}^{(n)}},$$

其中 \\(\epsilon\\) 是对抗边界，\\(\alpha\\) 是迭代步长，\\(\Pi_{x}^{\epsilon}\\) 是 clip 函数，确保 \\(||\hat{x}^{(n+1)} - x||\_{\infty} \le \epsilon\\) 且 \\(x\\) 中的像素都处于区间 \\([0, 1]\\)，具体算法如下。  

![Alg 1](/images/2021/PRN29/A1.png)

![Fig 2](/images/2021/PRN29/2.png)

## 最远负样本攻击 (Furthest-Negative Attack, FNA)

有许多额外图像可以获取的情况下。  

应当确保被攻击的特征向另一 ID 的簇靠近，且与其原 ID 的簇距离最远，因而这里旨在将被攻击的特征推往最远的簇。  

组合多个推动指导以及来自最远簇的拉动指导。  


$$\hat{x}^{(0)} = x,$$
$$\hat{x}^{(n+1)} = \Pi_{x}^{\epsilon}(\hat{x}^{(n)} + \alpha \cdot sign(\Delta_{n}^{FNA})),$$
$$\Delta_{n}^{FNA} = \sum_{g_{push} \in I_{\mathcal{l}(x)} \cup A} \frac{\partial D(f_{\hat{x}^{(n)}}, f_{g_{push}})}{\partial \hat{x}^{(n)}} - \sum_{g_{pull} \in I_{l_{far}} \cup A} \frac{\partial D(f_{\hat{x}^{(n)}}, f_{g_{pull}})}{\partial \hat{x}^{(n)}},$$

其中 \\(A\\) 是可获取的图像集合。  

$$l_{far} = argmax_{i \in \mathcal{l}(A)} D(f_{x}, \sum_{g \in I_{i} \cup A} \frac{f_{g}}{|I_{i} \cup A|}),$$

是最远簇的 ID。具体算法如下。  

![Alg 2](/images/2021/PRN29/A2.png)

## 指导采样在线对抗训练

![Alg 3](/images/2021/PRN29/A3.png)
