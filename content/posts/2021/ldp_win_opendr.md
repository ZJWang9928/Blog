---
title: "炼丹杂记 -- Windows 下使用 pip 安装 opendr"
date: 2021-09-12T15:35:04+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pip", "python", "opendr", "windows"]
draft: false
---

## 问题描述

直接使用 `pip install opendr` 会报一个 `failed with exit code 1181` 的错误。  

## 解决办法

首先安装 `glfw`，是 opengl 的一个框架：  

```
pip install glfw
```

在[这里](https://github.com/polmorenoc/opendr)下载 opendr 后构建并安装：  

```
> git clone https://github.com/polmorenoc/opendr.git
> cd ./opendr/opendr
> python setup.py build
> python setup.py install
```

不出意外的话要改已经安装成功了。  
