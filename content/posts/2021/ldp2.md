---
title: "炼丹杂记 -- nvidia-smi指令报错：Failed to initialize NVML 解决方案"
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "nvidia", "nvidia-smi", "python"]
date: 2021-01-22T14:27:20+08:00
draft: false
---

## 问题描述
使用指令  
```
nvidia-smi
```
时报错：
```
Failed to initialize NVML: Driver/library version mismatch
```

## 问题原因
NVIDIA 内核驱动版本与系统驱动不一致。  

## 解决办法
网上有很多调整驱动版本的方法教程，但其实最简单的方法只需要**重启一下**就能解决了。  
