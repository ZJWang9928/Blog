---
title: "Latex 公式中的空格表示"
date: 2021-06-21T14:11:57+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "latex", "paperwriting"]
draft: false
---

| 类型           | 代码       | 效果             | 备注              |
|----------------|------------|------------------|-------------------|
| 两个 quad 空格 | a \qquad b | \\(a \qquad b\\) | 两个 M 的宽度     |
| quad 空格      | a \quad b  | \\(a \quad b\\)  | 一个 M 的宽度     |
| 大空格         | a\ b       | \\(a \\ b\\)     | 1/3 M 的宽度      |
| 中等空格       | a\\;b      | \\(a \\; b\\)    | 2/7 M 的宽度      |
| 小空格         | a\\,b      | \\(a \\, b\\)    | 1/6 M 的宽度      |
| 没有空格       | ab         | \\(ab\\)         |                   |
| 紧贴           | a\\!b       | \\(a \\! b\\)    | 缩进 1/6 M 的宽度 |
