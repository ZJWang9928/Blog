---
title: "BLWL[15] Vim Tips"
date: 2020-01-23T22:37:58+08:00
categories: ["Better Life With Linux"]
tags: ["Vim", "Linux"]
draft: false
---
+ i 如 ci"，di<，yi( 等，在相应符号间执行有关操作
+ f 查询与其他操作配合使用实现操作至某字符的效果，如df:，yfa
+ visual 模式选中后输入 :normal 可进行批量操作
+ Ctrl+v 进入可视块模式  
+ Spell Check 开启情况下在错误词上 z= 进入备选词页面
+ Spell Check 用 ]s 和 [s 上下切换
+ Ctrl+o 回到上一次光标位置
+ Ctrl+i 回到上一次的上一次光标位置
+ gf 打开文件
+ :%TOhtml 将当前文件转为 HTML
+ Ctrl+a/Ctrl+x 数字增减
+ "+y 复制到系统剪贴板
+ "a(b,c,etc.)y/"a(b,c,etc.)p 指定寄存器复制粘帖，大写字母可在寄存器后追加内容
+ 在空白行使用 dip 命令可以删除所有临近的空白行
+ 在空白区使用 viw 可以选择所有空白字符
+ zz 重绘屏幕并把当前行显示在窗口正中间
