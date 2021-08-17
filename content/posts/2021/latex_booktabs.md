---
title: "Latex 表格使用 toprule、midrule 及 bottomrule 时出现 undefined control sequence 报错解决方案"
date: 2021-08-17T20:24:16+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "acm", "latex", "paperwriting"]
draft: false
---


## 问题描述

在 Latex 表格中，使用 \toprule、\midrule 以及 \bottomrule 时报错 “undefined control sequence”。  

## 解决办法

缺少相应宏包，导入即可：  

```
\usepackage{booktabs}
```
