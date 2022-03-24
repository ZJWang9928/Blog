---
title: "Latex 使用 thanks 脚注后多出一个空白页问题解决办法"
date: 2022-03-24T14:50:13+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "acm", "latex", "paperwriting"]
draft: false
---

## 问题描述

在 LaTex 中使用如下命令添加作者及其单位：  

```
\author{A, B, C}
\thanks{A is xxxxxx,
B is xxxxxx,
C is xxxxxx}
```

编译后发现在文档最开头多出了一个空白页。  

## 原因分析

`\thanks` 放在 `\author` 命令外面，导致其单独成页。  

## 解决办法

改成如下形式即可：  

```
\author{A, B, C
\thanks{A is xxxxxx,
B is xxxxxx,
C is xxxxxx}}
```

再编译就没有空白页了。  
