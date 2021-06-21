---
title: "BLWL[40] Manjaro/Archlinux “...” 的签名是未知信任的 问题解决方案"
date: 2021-06-21T19:47:15+08:00
categories: ["Better Life With Linux"]
tags: ["pacman", "Linux", "Notes", "KDE", "Manjaro", "ArchLinux"]
draft: false
---

## 问题描述

Manjaro/ArchLinux 使用 pacman 更新时遇到如下形式的问题：  


```
> sudo pacman -Syu
...
...
(748/748) 正在检查密钥环里的密钥                   [######################] 100%
(748/748) 正在检查软件包完整性                     [######################] 100%
错误：xxxxx: 来自 "xxxxxxxxxx" 的签名是未知信任的
:: 文件 xxxxxxxxxx 已损坏 (无效或已损坏的软件包 (PGP 签名)).
打算删除吗？ [Y/n] y
错误：无法提交处理 (无效或已损坏的软件包)
发生错误，没有软件包被更新。
```
## 原因分析
签名错误，可以通过重新生成签名来解决这一问题。  

## 解决办法

### 1. 更新一下 archlinux 密钥

#### 若尚未安装 archlinux-keyring

```
sudo pacman -S archlinux-keyring
```

#### 更新密钥

```
sudo pacman-key --refresh-keys
```

### 2. 重新加载签名密钥

```
sudo pacman-key --init
sudo pacman-key --populate
```

### 3. 清除pacman 的缓冲文件

```
sudo pacman -Scc
```

### 4. 再次更新

```
sudo pacman -Syu
```

不出意外的话问题已经解决了。  
