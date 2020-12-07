---
title: "BLWL[34] Vim 设置背景透明"
date: 2020-12-08T04:55:03+08:00
categories: ["Better Life With Linux"]
tags: ["Manjaro", "Linux"]
draft: true
---

Vim 通过 colorscheme 设置了主题后，通常背景会变成实色，想要让 Vim 的背景色跟随终端的透明度配置，在 .vimrc 文件中加入如下一行即可：  

```
hi Normal ctermfg=252 ctermbg=none
```
