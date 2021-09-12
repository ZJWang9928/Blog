---
title: "炼丹杂记 -- Windows 环境下 Python 2 可用的一个 Tensorflow 轮子"
date: 2021-09-12T20:39:13+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python2", "python", "tensorflow", "linux", "windows", "pip"]
draft: false
---

Tensorflow 在 Windows 环境下不支持 Python 2，但是最近需要在一台 Windows 系统的工作站上给基于 Python 2.7 的虚拟环境配置 Tensorflow。  

记录一下找到的一个可用的轮子：  

[https://github.com/fo40225/tensorflow-windows-wheel](https://github.com/fo40225/tensorflow-windows-wheel)

下载需要的 whl 文件后 pip install 即可。  

看下来支持到 1.10.0 版本。  
