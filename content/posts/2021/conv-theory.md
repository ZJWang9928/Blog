---
title: "炼丹杂记 -- 卷积定理"
date: 2021-06-24T21:23:26+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "convolution", "frequency", "DIP"]
draft: false
---

## 卷积定义

用算子 \\(\star\\) 表示两个函数的卷积，定义为  

$$(f \star h)(t) = \int_{-\infty}^{\infty} f(\tau)h(t-\tau)d\tau.$$

## 卷积定理

空间域中两个函数的卷积的傅里叶变换，等于频率域中两个函数傅里叶变换的乘积。反过来，如果有两个变换的乘积，那么可以通过计算傅里叶反变换得到空间域的卷积，换句话说，\\(f \star h\\) 和 \\(F \cdot H\\) 是一个傅里叶变换对。这一结果是卷积定理的一半，写为：  

$$(f \star h)(t) \Leftrightarrow (H \cdot F)(\mu),$$

双箭头表明右侧表达式是通过取左侧表达式的傅里叶正变换得到的，而左侧表达式是通过取右侧表达式的傅里叶反变换得到的。  

类似可以推出另外一半卷积定理：  

$$(f \cdot h)(t) \Leftrightarrow (H \star F)(\mu).$$

卷积定理是频率域滤波的基础。  
