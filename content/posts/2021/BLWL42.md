---
title: "BLWL[42] Manjaro pacman 更新提示 failed to synchronize all databases 解决方案"
date: 2021-09-03T15:05:14+08:00
categories: ["Better Life With Linux"]
tags: ["pacman", "Linux", "Notes", "pacman", "Manjaro", "ArchLinux"]
draft: false
---

## 问题描述

在运行 sudo pacman -Sy 进行更新时遇到如下报错：  

```
:: 正在同步软件包数据库...
错误：failed to synchronize all databases (无法锁定数据库)
```

## 问题分析

Pacman 数据库因之前的操作异常而被锁定，需要手动解锁。  

## 解决办法

执行如下命令：  

```
sudo rm -f /var/lib/pacman/db.lck
```

进而重新执行更新命令即可。  
