---
title: "BLWL[39] yay 遇到 Error: Rate limit reached 问题解决办法"
date: 2021-06-11T11:39:03+08:00
categories: ["Better Life With Linux"]
tags: ["yay", "Linux", "Notes", "KDE", "Manjaro", "ArchLinux"]
draft: false
---

在使用 `yay -S <软件名>` 安装时遇到如下错误：  

```
Error: Rate limit reached
```

尝试后发现一个有效的替代命令可以解决这个问题：  

```
pamac build <软件名>
```

特此记录一下，具体缘由与分析后续更新。  
