---
title: "Latex 使用 longtable 实现多页显示长表格"
date: 2021-08-25T12:41:56+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "longtable", "latex", "paperwriting"]
draft: false
---

在使用 Latex 写论文时，有时会出现表格过长超出一页的情况，可以通过 longtable 包实现表格的多页显示。  

### 首先引入宏包

```
\usepackage{longtable}
```

### 使用 longtable

首先删去 `\begin{table}` 和 `\end{table}`，再将 `tabular` 替换为 `longtable` 即可，`longtable` 嵌套于 `table` 环境之内会导致 `longtable` 无法分页，需要居中的话可以在 `longtable` 外套一层 `center` 环境。  

### 编译卡住问题的处理

需要在 `caption` 和 `label` 的最后都加上两个反斜杠 `\\` 再进行编译。  

### 每页均显示表头

为了让跨页表格的每页都有相应的表头和表尾，需要在 `\begin{longtable}` 之后插入如下内容：  

```
\endfirsthead
% 第一页表头内容

\endhead
% 跨页表头内容

\endfoot
% 跨页表尾内容

\endlastfoot
% 最后一页表尾内容
```
