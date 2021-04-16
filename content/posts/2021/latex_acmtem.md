---
title: "ACM 会议论文模板去除 ACM Reference Format 信息"
date: 2021-04-16T18:48:00+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "acm", "latex", "paperwriting"]
draft: false
---

在投稿撰文时，可以去掉 ACM 的 latex 模板中会有的 ACM Reference Format 信息。  

方法如下：  

在 `\documentclass[sigconf]{acmart}` 下面添加以下几行：  

```
\settopmatter{printacmref=false} % Removes citation information below abstract
\renewcommand\footnotetextcopyrightpermission[1]{} % removes footnote with conference information in first column
\pagestyle{plain} % removes running headers
```
