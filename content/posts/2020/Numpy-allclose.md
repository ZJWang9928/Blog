---
title: "Numpy散记 -- allclose函数的使用"
date: 2020-04-21T13:09:31+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "numpy", "python", "notes"]
draft: false
---

# 函数原型
> numpy.allclose(a, b, rtol=1e-05, atol=1e-08, equal\_nan=False)

# 参数
+ **a**, **b**：用于比较的两个输入数组
+ **rtol**：float型，相对容忍系数（relative tolerance parameter）
+ **atol**：float型，绝对容忍系数（absolute tolerance parameter）
+ **equal_nan**：bool型，是否在比较时认为NaN相等，若设为True，数组a中的使用NaN会被认为与数组b中的NaN相等

# 功能
np.allclose()函数用于比较给定数组的各对应元素是否都在一定限度内足够接近。   

容忍系数是**正数**，通常较小。相对差异（rtol * abs(b)）和绝对差异（atol）的和用来和a与b之差的绝对值进行比较，即若所有对应元素都满足     

$$|a - b| \le atol + rtol \cdot |b|,$$

则函数返回True。      

显然，上式对于数组a和b并不对称，所以有时`np.all_close(a, b)`的返回值可能与`np.all_close(b, a)`不同。      

数组a与b的比较适用广播原则，因此两数组的形状不一定需要一样。    

# 例子
    
    >>> np.allclose([1e10,1e-7], [1.00001e10,1e-8])
    False
    >>> np.allclose([1e10,1e-8], [1.00001e10,1e-9])
    True
    >>> np.allclose([1e10,1e-8], [1.0001e10,1e-9])
    False
    >>> np.allclose([1.0, np.nan], [1.0, np.nan])
    False
    >>> np.allclose([1.0, np.nan], [1.0, np.nan], equal_nan=True)
    True

    >>> array1 = np.array([0.12,0.17,0.24,0.29])
    >>> array2 = np.array([0.13,0.19,0.26,0.31])
    >>> np.allclose(array1,array2,0.1)  # 在0.1的容忍度下返回False
    False
    >>> np.allclose(array1,array2,0.2)  # 在0.2的容忍度下返回True
    True
