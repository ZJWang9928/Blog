---
title: "炼丹杂记 -- GELU 激活函数的 Pytorch 实现"
date: 2021-03-25T17:48:10+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "gelu", "python"]
draft: false
---

Transformers 中提出了一个新的激活函数 GELU，由于比较新，该模块仅在 1.8 以上版本的 Pytorch 中被收录。  
要在较低版本的 Pytorch 中使用 GELU，可自行编写实现，代码如下：  

```
import torch
import torch.nn as nn
import torch.nn.functional as F
import numpy as np
from matplotlib import pyplot as plt

class GELU(nn.Module):
    def __init__(self):
        super(GELU, self).__init__()

    def forward(self, x):
        return 0.5*x*(1+F.tanh(np.sqrt(2/np.pi)*(x+0.044715*torch.pow(x,3))))


def gelu(x):
    return 0.5*x*(1+np.tanh(np.sqrt(2/np.pi)*(x+0.044715*np.power(x,3))))
```
