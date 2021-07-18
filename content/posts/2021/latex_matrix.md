---
title: "Latex 中矩阵的几种实现方式"
date: 2021-07-18T16:20:27+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "latex", "paperwriting"]
draft: false
---

记录一下 Latex 中实现矩阵的集中方式。  

### 1. 无括号

#### 代码

```
\begin{matrix} 0 & 1 \\ 2 & 3 \end{matrix}
```

#### 效果

$$\begin{matrix} 0 & 1 \\\\ 2 & 3 \end{matrix}$$

### 2. 圆括号

#### 代码

```
\begin{pmatrix} 0 & 1 \\ 2 & 3 \end{pmatrix}
% 或
\left(\begin{matrix} 0 & 1 \\ 2 & 3 \end{matrix}\right)
```

#### 效果

$$\begin{pmatrix} 0 & 1 \\\\ 2 & 3 \end{pmatrix}$$

### 3. 方括号

#### 代码

```
\begin{bmatrix} 0 & 1 \\ 2 & 3 \end{bmatrix}
% 或
\left[\begin{matrix} 0 & 1 \\ 2 & 3 \end{matrix}\right]
```

#### 效果

$$\begin{bmatrix} 0 & 1 \\\\ 2 & 3 \end{bmatrix}$$

### 4. 花括号

#### 代码

```
\begin{Bmatrix} 0 & 1 \\ 2 & 3 \end{Bmatrix}
% 或
\left\{\begin{matrix} 0 & 1 \\ 2 & 3 \end{matrix}\right\}
```

#### 效果

$$\begin{Bmatrix} 0 & 1 \\\\ 2 & 3 \end{Bmatrix}$$

### 5. 竖线 

#### 代码

```
\begin{vmatrix} 0 & 1 \\ 2 & 3 \end{vmatrix}
% 或
\left|\begin{matrix} 0 & 1 \\ 2 & 3 \end{matrix}\right|
```

#### 效果

$$\begin{vmatrix} 0 & 1 \\\\ 2 & 3 \end{vmatrix}$$

### 6. 双竖线 

#### 代码

```
\begin{Vmatrix} 0 & 1 \\ 2 & 3 \end{Vmatrix}
```

#### 效果

$$\begin{Vmatrix} 0 & 1 \\\\ 2 & 3 \end{Vmatrix}$$
