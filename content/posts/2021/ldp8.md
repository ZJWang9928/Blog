---
title: "炼丹杂记 -- 时域抽样定理"
date: 2021-07-06T11:24:56+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "convolution", "frequency", "DIP", "digital signal processing"]
draft: false
---

## 时域抽样定理

设 \\(X(j\omega)\\) 和 \\(X(e^{j\Omega})\\) 分别表示连续时间信号 \\(x(t)\\) 和离散时间信号 \\(x[k]\\) 的频谱，即  

$$X(j\omega) = \int_{-\infty}^{\infty}x(t)e^{-j\omega t}dt, \ X(e^{j\Omega}) = \sum_{-\infty}^{\infty} x[k]e^{-j\Omega k}.$$  

若存在  

$$x[k] = x(t)|\_{t = kT},$$  

则有  

$$X(e^{j\Omega}) = \frac{1}{T}\sum_{-\infty}^{+\infty} X[j(\omega - n\omega_{sam})], \quad \omega_{sam} = 2\pi/T, \quad \Omega = \omega T.$$  

表明信号在时域的离散化，对应其频谱的周期化。  

其中，\\(T\\) 为抽样间隔，\\(\omega_{sam}\\) 为抽样角频率。  
