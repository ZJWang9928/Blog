---
title: "BLWL[26] Axel Command Tips"
date: 2020-03-12T11:00:10+08:00
categories: ["Better Life With Linux"]
tags: ["Linux", "Command", "notes"]
draft: false
---

axel 是 Linux 下一个不错的 HTTP/ftp 高速下载工具。   
    
axel 支持多线程下载、断点续传，且可以从多个地址或者从一个地址的多个连接来下载同一个文件。   
    
下载过程中断可以再执行下载命令恢复上次的下载进度。    
    
    
### 语法
    
> axel [options] url1 [url2] [url...]
    
### 选项
    
> --max-speed=x , -s x         # 最高速度 x  
> --num-connections=x , -n x   # 连接数 x  
> --output=f , -o f            # 下载为本地文件 f  
> --search[=x] , -S [x]        # 搜索镜像  
> --header=x , -H x            # 添加头文件字符串 x（指定 HTTP header）  
> --user-agent=x , -U x        # 设置用户代理（指定 HTTP user agent）   
> --no-proxy ， -N             # 不使用代理服务器  
> --quiet ， -q                # 静默模式  
> --verbose ，-v               # 更多状态信息  
> --alternate ， -a            # Alternate progress indicator  
> --help ，-h                  # 帮助  
> --version ，-V               # 版本信息  
    
### 实例
如下载 lnmp 安装包指定 10 个线程，存到 /tmp/：   
> axel -n 10 -o /tmp/ http://www.linuxde.net/lnmp.tar.gz
