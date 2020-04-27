---
title: "BLWL[9] Unzip解压中文乱码问题解决方案"
date: 2020-01-23T22:31:58+08:00
categories: ["Better Life With Linux"]
tags: ["Manjaro", "Linux"]
draft: false
---
1.执行unzip命令，查看系统上是否有unzip，若有直接卸载  

        yaourt -Rns unzip

2.安装unzip-icon

        yaourt unzip-icon

3.指定编码解压文件
        
        unzip -O GBK ZIP_file


