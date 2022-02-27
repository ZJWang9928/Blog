---
title: "BLWL[43] Linux 命令出现 Argument List Too Long 问题的两种解决办法"
date: 2022-02-27T15:47:04+08:00
categories: ["Better Life With Linux"]
tags: ["pacman", "Linux", "Notes", "pacman", "Manjaro", "ArchLinux"]
draft: false
---

## 问题描述

Linux 下在使用 `cp`、`mv`、`rm` 等命令时出现 `Argument list too long` 错误，命令执行失败。  

## 问题原因

命令涉及的文件数目过多，导致命令参数列表过长，无法执行。  

## 解决办法

这里记录两种可行的解决办法，以文件复制为例，将 dir1 目录下文件名后缀为 jpg 的文件复制到 dir2 目录。

### 方法 1

```
find dir1/ -name ".jpg" | xargs -i cp {} dir2/
```

**注**：用 `find` 命令查找相应文件。`xargs` 命令可以将参数传递给其他命令，通过 `-i` 将 `xargs` 的内容赋值给 `{}`。  

### 方法 2

```
find dir1/ -name ".jpg" -exec cp {} dir2/ \;
```

**注**：同样先用 `find` 命令查找相应文件，`-exec` 用于指明后续执行的命令，而 `-exec` 以分号结尾，由于不同环境下分号的意义可能不同，故对其进行转义。  
