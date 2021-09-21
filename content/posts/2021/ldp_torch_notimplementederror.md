---
title: "炼丹杂记 -- PyTorch 出现 NotImplementedError 报错的一种可能情况"
date: 2021-09-21T23:36:45+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "coding", "python"]
draft: false
---

## 问题描述

PyTorch 一使用网络进行计算即出现如下报错：  

```
raise NotImplementedErrorh
```

## 问题原因

经过反复的调试和检查后，发现是 forward 函数的问题，网络模型类中的 `def forward` 一行多缩进了一个 Tab 位，导致 forward 函数无法被检测到......  

修改后问题解决。  

所以说 Python 代码块的对齐和缩进问题请务必细致......  

比较尴尬的一个失误，记录一下以供参考。  
