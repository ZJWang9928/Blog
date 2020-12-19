---
title: "BLWL[37] 简单干净卸载 cuda 的方法"
date: 2020-12-19T21:00:26+08:00
categories: ["Better Life With Linux"]
tags: ["Ubuntu", "cuda", "drivers", "Nvidia", "Linux", "cv"]
draft: false
---

网上有各种有关如何通过一系列命令卸载 cuda 的教程，但其实 cuda 是自带卸载脚本的。  

## 第一步： 找到 cuda 所在路径
cuda 位于 `/usr/local/cuda/bin` 目录下。  

进入目录后运行卸载脚本：  

```
sudo ./uninstall_cuda_10.0.pl
```

## 第二步： 删除 cuda 目录

```
sudo rm -rf cuda-10.0
```
