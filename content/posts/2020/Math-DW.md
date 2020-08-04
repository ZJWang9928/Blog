---
title: "考研数学复习杂笔记 -- 多元函数微分学"
date: 2020-04-20T16:19:26+08:00
categories: ["NJTU CST Learning Notes"]
tags: ["notes", "math"]
draft: false
---

+ **多元函数可导推不出连续与可微**：多元的可导是指**一阶偏导数**存在，而偏导数是用**一元函数极限**定义的，只与点\\((x\_{0}, y\_{0})\\)邻域内过该点且平行于两坐标轴的十字架方向函数值有关；而连续和可微都是用**重极限**定义的，其动点\\((x, y)\\)是以任意方式趋于\\((x\_{0}, y\_{0})\\)，与点\\((x\_{0}, y\_{0})\\)邻域内函数值有关。       
+ 若\\(\frac{\partial P}{\partial y}\\)和\\(\frac{\partial Q}{\partial x}\\)在区域\\(D\\)上连续，且\\(P(x,y)dx + Q(x,y)dy\\)在\\(D\\)上是某二元函数**全微分**，则在\\(D\\)上\\(\frac{\partial P}{\partial y} \equiv \frac{\partial Q}{\partial x}\\)，这个结论可以直接用。

+ 由\\(\frac{\partial u}{\partial \theta} \equiv 0\\)，可知\\(u\\)与\\(\theta\\)无关。

+ 线性函数二阶导数为零。

+ 虽然方向导数计算公式\\(\frac {\partial z}{\partial l} = \frac{\partial z}{\partial x}cos\alpha + \frac{\partial z}{\partial y}cos \beta\\)中出现了函数在该点的两个一阶偏导数，但若**偏导数之一不存在**，由此并**不能断言函数在该点沿任一方向的方向导数不存在**，如\\(z = \sqrt{x^{2}+y^{2}}\\)。
