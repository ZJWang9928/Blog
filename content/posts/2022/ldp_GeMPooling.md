---
title: "炼丹杂记 -- GeM Pooling 的 Pytorch 实现"
date: 2022-01-21T23:21:35+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "coding", "python", "pooling"]
draft: false
---

记录一下 Generalized Mean Pooling (GeM 池化) 的一种 PyTorch 实现方式。  

## 定义

GeM 池化公式定义如下：

$$f_{g, c} = (\frac{1}{hw} \sum_{(i, j)} f_{4, (c, i, j)}^p)^{1/p}\_{c = 1, 2, \dots, C},$$

其中超参数 \\(p > 0\\)，默认设为 3.0，\\(f_{g} \in \mathbb{R}^{C \times 1}\\)。  

## PyTorch 实现

```
class GeM(nn.Module):
    def __init__(self, p=3, eps=1e-6):
        super(GeM,self).__init__()
        self.p = nn.Parameter(torch.ones(1)*p)
        self.eps = eps

    def forward(self, x):
        return self.gem(x, p=self.p, eps=self.eps)
        
    def gem(self, x, p=3, eps=1e-6):
        return F.avg_pool2d(x.clamp(min=eps).pow(p), (x.size(-2), x.size(-1))).pow(1./p)
        
    def __repr__(self):
        return self.__class__.__name__ + '(' + 'p=' + '{:.4f}'.format(self.p.data.tolist()[0]) + ', ' + 'eps=' + str(self.eps) + ')'
```

[参考链接传送门](https://amaarora.github.io/2020/08/30/gempool.html#pytorch-implementation)
