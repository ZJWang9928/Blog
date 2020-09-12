---
title: "BLWL[27] Shell 杂记"
date: 2020-04-13T10:06:16+08:00
categories: ["Better Life With Linux"]
tags: ["Linux", "Command", "notes"]
draft: false
---

+ **cd -** 回到前一个工作路径
+ 若在输入命令时中途改变了主意，可以按下** Alt-#* *在行首添加 # 以注释该命令并回车执行（相当于依次按下 **Ctrl-A, #, Enter**）。这样做的话，之后借助命令行历史记录，你可以很方便恢复你刚才输入到一半的命令
+ **pstree -p** 以一种优雅的方式展示进程树
+ **pgrep**和**pkill** 可以**名字**查找进程或发送信号（-f 参数通常有用）
+ **lsof** 用于查看开启的套接字文件
+ **uptime**或**w** 用于查看系统已经运行多长时间
+ 使用 **man ascii** 查看具有十六进制和十进制值的 ASCII 表。**man unicode**，**man utf-8**，以及 **man latin1** 有助于了解通用的编码信息
