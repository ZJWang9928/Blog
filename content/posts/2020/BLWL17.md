---
title: "BLWL[17] Vim保存上次退出时光标位置"
date: 2020-01-23T22:39:58+08:00
categories: ["Better Life With Linux"]
tags: ["Vim", "Linux"]
draft: false
---

记录上次关闭某一文件时的光标位置，并在下一次打开该文件时将光标移动到该位置  
在 .vimrc 中添加
    
    au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif

保存即可
