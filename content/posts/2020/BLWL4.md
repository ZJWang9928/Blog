---
title: "BLWL[4] Manjaro KDE上Teamviewer闪退问题解决方案"
date: 2020-01-23T22:26:58+08:00
categories: ["Better Life With Linux"]
tags: ["Manjaro", "Linux"]
draft: false
---

直接 pacman 安装的 TV 会出现一直 "Not Ready,..." 的情况  
此时先  

	sudo teamviewer --daemon enable

再打开 Teamviewer 即可
