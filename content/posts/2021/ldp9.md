---
title: "炼丹杂记 -- Nyquist 频率与 Nyquist 间隔"
date: 2021-07-06T11:36:55+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "convolution", "frequency", "DIP", "digital signal processing"]
draft: false
---

若[带限信号](http://jonathanwayy.xyz/2021/ldp6/) \\(x(t)\\) 的最高角频率为 \\(\omega_{m}\\)，则在一定条件下，信号 \\(x(t)\\) 可以用等间隔 \\(T\\) 的抽样值唯一表示。  

抽样间隔 \\(T\\) 需满足：  

$$T \le \frac{\pi}{\omega_{m}} = \frac{1}{2f_{m}},$$  

或者  

$$ f_{sam} \ge 2f_{m}, \quad \omega_{sam} \ge 2\omega_{m}.$$

\\(f_{sam} = 2f_{m}\\) 是使抽样信号频谱不混叠时的最小抽样频率，称为奈奎斯特频率 (Nyquist Rate)。  

\\(T = 1/2f_{m}\\) 是使抽样信号频谱不混叠时的最大抽样间隔，称为奈奎斯特间隔。  
