---
title: "炼丹杂记 -- 去除 plt.savefig() 保存图像时的白边"
date: 2021-12-22T17:01:28+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "python", "plt", "linux"]
draft: false
---

在用 plt 保存图像时发现图像带有白边，查阅文档后解决了这一问题，在此记录一下。  

将语句写成如下形式即可：  

```
plt.savefig('xxx.png', dpi=500, bbox_inches='tight', pad_inches=0.0)
```

其中将 `pad_inches` 参数置零即可去除白边，而将 `bbox_inches` 设为 tight 则能够去除坐标轴占用的空间（仅设置将坐标轴可视化关闭仍会占据空间，只是不显示）。  
