---
title: "炼丹杂记 -- Python 2 安装 opencv-python 包出错解决办法"
date: 2021-09-11T21:46:05+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python2", "python", "opencv", "linux", "windows", "pip"]
draft: false
---

## 问题描述

在为使用 Python 2.7 的虚拟环境使用如下命令安装 OpenCV 时  

```
pip2 install opencv-python
```

出现了如下报错：  

```
TypeError: 'NoneType' object is not iterable
```

## 原因分析

出现这种情况是因为最新版的 OpenCV 不再支持 Python 2.7，而直接使用 pip 命令会下载最新版的 OpenCV。  

## 解决办法

安装时指定老版本，最后一个支持 Python 2.7 的 OpenCV 版本为 `4.2.0.32`：  

```
pip2 install opencv-python==4.2.0.32
```
