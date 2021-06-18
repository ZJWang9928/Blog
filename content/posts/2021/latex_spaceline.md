---
title: "Latex 插入空行"
date: 2021-06-16T20:38:50+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "latex", "paperwriting"]
draft: false
---

有时需要在 Latex 文档中插入一个空行，查阅一些文档并做了一些尝试后，发现了一种较为简单的方法：  

```
\\ \hspace*{\fill} \\
```

即换行，用空格填充后，再次换行。  
