---
title: "Numpy散记 -- clip函数的使用"
date: 2020-04-21T12:54:52+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "numpy", "python", "notes"]
draft: false
---

# 函数原型
> numpy.clip(a, a\_min, a\_max, out=None, **kwargs)     

# 参数
+ **a**：数组
+ **a\_max**：数组元素最大值
+ **a\_min**：数组元素最小值

# 功能
np.clip()函数用于将数组元素的值保持在给定区间内，超出给定区间的元素值被裁剪到区间边缘。     

# 例子

    >>> a = np.arange(10)
    >>> np.clip(a, 1, 8)
    array([1, 1, 2, 3, 4, 5, 6, 7, 8, 8])
    >>> a
    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
    >>> np.clip(a, 3, 6, out=a)
    array([3, 3, 3, 3, 4, 5, 6, 6, 6, 6])
    >>> a = np.arange(10)
    >>> a
    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
    >>> np.clip(a, [3, 4, 1, 1, 1, 4, 4, 4, 4, 4], 8)
    array([3, 4, 2, 3, 4, 5, 6, 7, 8, 8])
