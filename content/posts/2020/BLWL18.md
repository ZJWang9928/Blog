---
title: "BLWL[18] Vimscript Notes"
date: 2020-01-23T22:40:58+08:00
categories: ["Better Life With Linux"]
tags: ["Vim", "Linux"]
draft: false
---

+ :echo 命令输出的信息在脚本运行完毕后就会消失， :echom 打印的信息会保存下来，可以执行 :messages 命令再次查看
+ :set <name>/:set no<name> 或直接 :set <name>! 可切换布尔选项的值
+ :set <name>? 命令可获取一个布尔选项的当前值 
+ 可在一个 :set 命令中设置多个选项的值 
+ 键盘映射命令 :map 后无法使用注释
+ 可用 nmap、 vamp、 imap 分别指定三种模式下的映射
+ map 系列命令存在**递归危险**
+ map 系列命令对应的非递归映射： noremap、 nnoremap、 vnoremap、 inoremap
