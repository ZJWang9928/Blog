---
title: "KMP 算法笔记 - Next、Nextval数组求法、失配回退及复杂度"
date: 2020-08-04T16:55:15+08:00
categories: ["C/C++ And Algorithm"]
tags: ["algorithm", "note", "kmp"]
draft: false
---

## Next 数组求法

**1.** 字符串 str 元素下标从 1 开始；  

**2.** 令 Next[1] = 0，Next[2] = 1；  

**3.** 令 i = 3，判断 str[i-1] 与 str[Next[i-1]] 是否相等；  
**3.1** 若相等，则 Next[i] = Next[i-1] + 1；  
**3.2** 若不相等，则继续判断 str[i-1] 与 str[Next[Next[i-1]]] 是否相等，递归执行，直至回溯到 Next 数组头或相等为止。  

## Nextval 数组求法

**1.** 令 Nextval[1] = 0；  

**2.** 令 i = 2，判断 str[i] 与 str[Next[i]] 是否相等；  
**2.1** 若相等，则一直按 str[Next[Next[i]]] 式进行递归比较，直至不相等或到头；  
**2.2** 若不相等，则 Nextval[i] = Next[i]。  

## 失配回退

失配时，**i 的值不变**，**j = Next[j]**。  

## 算法复杂度

主串长度为 n，子串长度为 m。  

简单模式匹配算法复杂度为 **O(mn)**。  

KMP算法复杂度为 O(m + n)。  
