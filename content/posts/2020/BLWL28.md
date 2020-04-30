---
title: "BLWL[28] Manjaro无法启动vmware虚拟机问题"
date: 2020-04-30T10:22:14+08:00
categories: ["Better Life With Linux"]
tags: ["vmware", "Linux"]
draft: false
---

# 问题1
Could not open /dev/vmmon: ???.     
Please make sure that the kernel module `vmmon’ is loaded.      

## 解决方案
### 1.查看自己的内核版本

    > uname -r
    5.6.7-1-MANJARO

### 2.安装对应的linux-headers
    
    sudo pacman -S linux56-headers

### 3.加载内核模块
    
    sudo modprobe -a vmw_vmci vmmon

# 问题2
Failed to connect virtual device ‘Ethernet0’. More information can be found in the vmware.log file.     

## 原因分析
没有设置开机自启虚拟机网络      

## 解决方案

    > systemctl restart vmware-networks
    > systemctl enable vmware-networks
