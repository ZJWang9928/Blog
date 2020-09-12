---
title: "BLWL[21] Ack Command Tips"
date: 2020-01-23T22:43:58+08:00
categories: ["Better Life With Linux"]
tags: ["Linux"]
draft: false
---

ack 是一个使用 Perl 语言开发的高效代码搜索工具  
默认会搜索当前目录下所有文件内容，只要包含关键字就输出  
    
### 文本搜索
    
> ack keyword   
> ack -l keyword    # 只显示文件名   
> ack -i keyword    # 忽略大小写   
> ack -w keyword    # 强制要求 PATTERN 匹配整个单词   

### 查找文件
可以代替 find + grep 的功能  
-f 选项表示打印所有将要被搜索的文件，事实上不会执行搜索，如果后面加 PATTERN ，那么就在路径中搜索文件名  
-g 选项表示搜索当前路径下的符合 PATTERN 的文件  
--python 选项可指定在 Python 文件中搜索  
