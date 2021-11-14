---
title: "Latex 表格中绘制经过指定列的水平线"
date: 2021-11-13T14:47:13+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "latex", "paperwriting"]
draft: false
---

在用 Latex 绘制表格的过程中，有时需要绘制不完全贯穿整行的水平线，可用如下命令指定需要贯穿的列：  

```
\cline{起始列-终止列}
```

此命令中列从 1 开始排序，而非从 0 开始。  
