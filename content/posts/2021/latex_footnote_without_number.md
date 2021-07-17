---
title: "Latex 添加无编号脚注"
date: 2021-07-17T11:53:47+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "latex", "paperwriting"]
draft: false
---

在使用 Latex 的过程中，有时需要添加不带编号的脚注，目前尝试过有效的有两种办法，记录如下。  

### 方法一

```
\renewcommand{\thefootnote}{}
\footnotetext{脚注内容}
```

### 方法二

```
\let\thefootnote\relax\footnotetext{脚注内容}
\footnote{脚注内容}
\footnote{脚注内容}
\footnote{脚注内容}
...
```

第二种方法后续使用的 `\footnote{}` 都不带脚注。  
