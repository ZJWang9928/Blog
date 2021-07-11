---
title: "炼丹杂记 -- Channel Shuffle 操作的 PyTorch 实现"
date: 2021-07-11T14:26:18+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "attention", "python"]
draft: false
---

在 ShuffleNet、SA-Net 以及一系列模型中涉及到了一种 Channel Shuffle 操作，用于在沿着通道维分组运算后保证各组特征之间能够有信息交互。  

Channel Shuffle 的机制可以理解为“变形 —— 转置 —— 变形 (reshape -- transpose (permute) -- reshape)” 三个步骤下。  

具体的 PyTorch 实现如下：  

```
def channel_shuffle(x, group_num):
    b, c, h, w = x.shape

    # 变形
    x = x.reshape(b, group_num, -1, h, w)
    # 转置
    x = x.permute(0, 2, 1, 3, 4)
    # 变形
    x = x.reshape(b, -1, h, w)

    return x
```
