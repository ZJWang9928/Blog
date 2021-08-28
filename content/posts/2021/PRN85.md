---
title: "[论文阅读笔记 -- 频域] Frequency Separation for Real-World Super-Resolution (ICCVW 2019)"
date: 2021-08-27T13:40:16+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "image"]
draft: false
---

# 1911.07850 Frequency Separation for Real-World Super-Resolution (ICCVW 2019)

[开源代码传送门](https://github.com/ManuelFritsche/real-world-sr)

![Fig 1](/images/2021/PRN85/1.png)

## 速览

速读一篇文献，主要关注一下频域相关的处理。  

下采样操作去除了高频信息而保留低频信息。

因而本文对高低频信息进行了分离，进而分别处理，使用简单的线性滤波器实现，定义一个低通滤波器 \\(w_{l, d}\\)，通过卷积操作得到高低频信息：  

$$x_{L, d} = w_{L, d} \* x_{d},$$
$$x_{H, d} = x_{d} - x_{L, d} = (\delta - w_{L, d}) \* x_{d}.$$

故此高通滤波器定义为 \\(w_{H, d} = \delta - w_{L, d}\\)。  

![Fig 2](/images/2021/PRN85/2.png)

## 模型架构

![Fig 3](/images/2021/PRN85/3.png)

![Fig 4](/images/2021/PRN85/4.png)

## 滤波器代码

```
class GaussianFilter(nn.Module):
    def __init__(self, kernel_size=5, stride=1, padding=4):
        super(GaussianFilter, self).__init__()
        # initialize guassian kernel
        mean = (kernel_size - 1) / 2.0
        variance = (kernel_size / 6.0) ** 2.0
        # Create a x, y coordinate grid of shape (kernel_size, kernel_size, 2)
        x_coord = torch.arange(kernel_size)
        x_grid = x_coord.repeat(kernel_size).view(kernel_size, kernel_size)
        y_grid = x_grid.t()
        xy_grid = torch.stack([x_grid, y_grid], dim=-1).float()

        # Calculate the 2-dimensional gaussian kernel
        gaussian_kernel = torch.exp(-torch.sum((xy_grid - mean) ** 2., dim=-1) / (2 * variance))

        # Make sure sum of values in gaussian kernel equals 1.
        gaussian_kernel = gaussian_kernel / torch.sum(gaussian_kernel)

        # Reshape to 2d depthwise convolutional weight
        gaussian_kernel = gaussian_kernel.view(1, 1, kernel_size, kernel_size)
        gaussian_kernel = gaussian_kernel.repeat(3, 1, 1, 1)

        # create gaussian filter as convolutional layer
        self.gaussian_filter = nn.Conv2d(3, 3, kernel_size, stride=stride, padding=padding, groups=3, bias=False)
        self.gaussian_filter.weight.data = gaussian_kernel
        self.gaussian_filter.weight.requires_grad = False

    def forward(self, x):
        return self.gaussian_filter(x)


class FilterLow(nn.Module):
    def __init__(self, recursions=1, kernel_size=5, stride=1, padding=True, include_pad=True, gaussian=False):
        super(FilterLow, self).__init__()
        if padding:
            pad = int((kernel_size - 1) / 2)
        else:
            pad = 0
        if gaussian:
            self.filter = GaussianFilter(kernel_size=kernel_size, stride=stride, padding=pad)
        else:
            self.filter = nn.AvgPool2d(kernel_size=kernel_size, stride=stride, padding=pad, count_include_pad=include_pad)
        self.recursions = recursions

    def forward(self, img):
        for i in range(self.recursions):
            img = self.filter(img)
        return img


class FilterHigh(nn.Module):
    def __init__(self, recursions=1, kernel_size=5, stride=1, include_pad=True, normalize=True, gaussian=False):
        super(FilterHigh, self).__init__()
        self.filter_low = FilterLow(recursions=1, kernel_size=kernel_size, stride=stride, include_pad=include_pad,
                                    gaussian=gaussian)
        self.recursions = recursions
        self.normalize = normalize

    def forward(self, img):
        if self.recursions > 1:
            for i in range(self.recursions - 1):
                img = self.filter_low(img)
        img = img - self.filter_low(img)
        if self.normalize:
            return 0.5 + img * 0.5
        else:
            return img
```
