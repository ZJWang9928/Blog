---
title: "炼丹杂记 -- Python 2 安装 grpcio 的一个可用版本"
date: 2021-09-12T20:25:22+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python2", "python", "grpcio", "linux", "windows", "pip"]
draft: false
---

在一台 Windows 工作站上需要配置一个基于 Python 2.7 的 TF 虚拟环境，安装一个依赖包 grpcio 时遇到了问题。  

分析以后应该是由于新版本的 grpcio 不支持 Python 2.7 了，直接用 pip 安装不行，需要指定版本，这里记录一下一个可用的版本：  

```
pip2 install grpcio==1.36.1
```
