---
title: "BLWL[16] Vimdiff Tips"
date: 2020-01-23T22:38:58+08:00
categories: ["Better Life With Linux"]
tags: ["Vim", "Linux"]
draft: false
---

Vimdiff 是 Vim 自带的一个文件差异编辑器  
启动 vimdiff：
    
    vimdiff file1 file2

常用命令：  
+ ]c    下一差异
+ [c    上一差异
+ do    导入差异
+ dp    导出差异
+ zo    打开折叠
+ zc    关闭折叠
+ :diffupdate   重新扫描文件
