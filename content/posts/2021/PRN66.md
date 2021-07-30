---
title: "[论文阅读笔记 -- 频域] Wavelet-enhanced Convolutional Neural Network (2018)"
date: 2021-07-30T21:55:30+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# Wavelet-enhanced Convolutional Neural Network: A New Idea in A Deep Learning Paradigm (2018)

## 概览

结合 CNN 与小波变换提升分割任务表现。  

小波变换在图像处理中的主要作用在于其能够将图像分解为含有不同级别细节的不同尺度。  

使用 Pywt 实现小波变换，采用 the first order of Daubechies wavelet family (db1)，包含 approximate、horizontal、vertical、diagonal 四种形式。  

![Fig 1](/images/2021/PRN66/1.png)

![Fig 2](/images/2021/PRN66/2.png)

![Fig 3](/images/2021/PRN66/3.png)
