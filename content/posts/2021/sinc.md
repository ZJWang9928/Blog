---
title: "辛格函数 sinc 杂记"
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "sinc", "math"]
date: 2021-05-10T13:21:04+08:00
draft: false
---

sinc 函数在不同领域有不同的定义，用符号 \\(sinc(x)\\) 表示，可被定义为**归一化的**或者**非归一化的**，不过两种函数都是正弦函数和单调递减函数 \\(\frac{1}{x}\\) 的乘积。  

## 数字信号处理和通信理论中的归一化 \\(sinc\\) 函数

对于 \\(\forall x \neq 0, sinc(x) = \frac{sin(\pi x)}{\pi x}\\)。  

\\(sinc(0) = 1\\)，对于 \\(x\\) 的其他整数值， \\(sinc(x) = 0\\)。  

## 数学领域中曾使用的非归一化 \\(sinc\\) 函数

对于 \\(\forall x \neq 0, sinc(x) = \frac{sin(x)}{x}\\)。  
