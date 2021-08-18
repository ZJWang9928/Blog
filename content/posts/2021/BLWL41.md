---
title: "BLWL[41] Linux 通过 tar 进行系统备份与恢复"
date: 2021-08-18T14:36:27+08:00
categories: ["Better Life With Linux"]
tags: ["pacman", "Linux", "Notes", "tar", "backup", "Manjaro", "ArchLinux"]
draft: false
---

记录一下如何使用 tar 工具进行 Linux 系统的备份与恢复。  

### 1. 安装 tar 工具

```
> sudo pacman -S tar
```

### 2. 新建一个存放备份文件的目录

```
> mkdir -p backup_dir
```

`backup_dir` 目录可根据自己的设备情况选择。  

### 3. 执行下列命令进行备份

```
> tar -czvpf ./backup_dir/manjaro_bk.tar.gz --exclude=/sys --exclude=/lost+found --exclude=/dev --exclude=/media --exclude=/mnt --exclude=/proc /
```

`--exclude=...` 指定打包时需要排除在外的文件或目录。  

### 4. 初次备份成功后可以定期通过如下命令进行增量备份

```
> tar -uzvpf ./backup_dir/manjaro_bk.tar.gz --exclude=/sys --exclude=/lost+found --exclude=/dev --exclude=/media --exclude=/mnt --exclude=/proc /
```

### 5. 通过如下命令还原备份

```
> tar -zxvf ./backup_dir/manjaro_bk.tar.gz -C /
```
