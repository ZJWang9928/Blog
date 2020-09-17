---
title: "BLWL[9] Unzip解压中文乱码问题解决方案"
date: 2020-01-23T22:31:58+08:00
categories: ["Better Life With Linux"]
tags: ["Manjaro", "Linux"]
draft: false
---

在 Linux 下使用 unzip 解压文件时，解压完毕的文件中若包含中文内容，可能会出现乱码的情况，本文提供一个可供参考的解决方案。  

1.执行 unzip 命令，查看系统上是否有 unzip，若有直接卸载  

        yaourt -Rns unzip

2.安装 unzip-icon

        yaourt unzip-icon

3.指定编码解压文件
        
        unzip -O GBK ZIP_file


