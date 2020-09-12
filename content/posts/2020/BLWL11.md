---
title: "BLWL[11] 使用Clear命令Terminals Database Inaccessible报错问题解决方案"
date: 2020-01-23T22:33:58+08:00
categories: ["Better Life With Linux"]
tags: ["Manjaro", "Linux"]
draft: false
---
问题：  
当使用 clear 命令时出现如下报错  
   
        ~$ clear   
        terminals database is inaccessible   

临时解决方法：
    
    ~$ export TERMINFO=/usr/share/terminfo   
    
最好将上面这条 export 命令添加到 .bashrc 中。
