---
title: "BLWL[14] Vim Macro的使用"
date: 2020-01-23T22:36:58+08:00
categories: ["Better Life With Linux"]
tags: ["Vim", "Linux"]
draft: false
---
可以通过 Vim 的宏录制完成重复的同一操作  
1. 在 normal 模式下输入 qa（qb，qc，etc.）选择将录制好的宏放入寄存器 a（b，c，etc.）  
2. 正常情况下， Vim 的命令行会显示“开始录制”字样，此时即可开始进行操作，记得最后一步把光标移到下一行  
3. 在 normal 模式下输入 q，结束宏录制  
4. 在 normal 模式下输入 @a（@b，@c，etc.）执行相应宏， n@a 多次执行  
