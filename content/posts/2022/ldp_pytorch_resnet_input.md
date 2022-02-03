---
title: "炼丹杂记 -- Pytorch 修改官方预训练模型输入通道数"
date: 2022-02-03T15:48:26+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "pytorch", "coding", "python", "resnet"]
draft: false
---

有时需要修改预训练模型的输入通道数，在此记录一下一种可行的方法。  

首先加载需要修改的预训练模型，查看一下模型的第一层，以 ResNet-50 为例：  

```
import torchvision.models as models
backbone = models.resnet101(pretrained=False)
print(backbone.conv1)
```

得到的输出如下：  

```
Conv2d(3, 64, kernel_size=(7, 7), stride=(2, 2), padding=(3, 3), bias=False)
```

可以看出此时的输入通道数为 3，只需保持其他参数然后修改输入通道数重新为该层赋值即可：  
```
backbone.conv1= nn.Conv2d(9, 64, kernel_size=7, stride=2, padding=3, bias=False)
```
