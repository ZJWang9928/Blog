---
title: "BLWL[33] Manjaro Linux 下 Docker 的安装与使用"
date: 2020-09-14T22:17:35+08:00
categories: ["Better Life With Linux"]
tags: ["docker", "manjaro", "Linux", "notes"]
draft: false
---

## 背景知识

Docker 是一个开源的应用容器引擎，基于 Go 语言并遵从 Apache 2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iOS 的 app）,更重要的是容器性能开销极低。

Docker 包括三个基本概念:  

**镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。  

**容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。  

**仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。  

Docker 使用客户端-服务器 (C/S) 架构模式，使用远程 API 来管理和创建 Docker 容器。  

Docker 容器通过 Docker 镜像来创建。

容器与镜像的关系类似于面向对象编程中的对象与类。  

| Docker | 面向对象 |
|--------|----------|
| 镜像   | 类       |
| 容器   | 对象     |

本文介绍 Docker 在 Manjaro Linux 环境下的安装及使用。  

## Docker 安装与启动

首先更新一下系统环境

    $ sudo pacman -Syu

然后安装 Docker

    $ sudo pacman -S docker

启动 Docker 服务

    $ sudo systemctl start docker.service 

可以把 Docker 添加到启动项中，使其能随系统启动而自动运行

    $ sudo systemctl enable docker.service
    Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.

查看版本信息，从而确认 Docker 是否已经成功安装
    
    $ sudo docker version
    Client:
     Version:           19.03.12-ce
     API version:       1.40
     Go version:        go1.14.5
     Git commit:        48a66213fe
     Built:             Sat Jul 18 01:33:21 2020
     OS/Arch:           linux/amd64
     Experimental:      false

    Server:
     Engine:
      Version:          19.03.12-ce
      API version:      1.40 (minimum version 1.12)
      Go version:       go1.14.5
      Git commit:       48a66213fe
      Built:            Sat Jul 18 01:32:59 2020
      OS/Arch:          linux/amd64
      Experimental:     false
     containerd:
      Version:          v1.4.0.m
      GitCommit:        09814d48d50816305a8e6c1a4ae3e2bcc4ba725a.m
     runc:
      Version:          1.0.0-rc92
      GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19403a7aff
     docker-init:
      Version:          0.18.0
      GitCommit:        fec3683

此时，说明 Docker 已成功安装。  

可以通过如下命令查看当前有多少正在运行的 Docker 容器及其相关的配置信息

    $ sudo docker info

## 无需 root 权限运行 Docker

默认情况下， Docker 命令的运行需要 root 权限，必须带上 sudo 或切换到超级用户下进行相关操作。  

可以通过**将当前用户添加到 Docker 组中**来避免使用 sudo：  

    $ sudo usermod -aG docker $USER

输入该命令后，需要**重启系统**或**注销然后重新登陆**（推荐后者）生效，接着就可以直接执行 Docker 命令了。  

## 改用国内镜像

Docker 默认使用的是外国源，访问速度很慢而且很容易断线，为此我们可以使用国内的镜像来代替默认的源。  

打开或创建`/etc/docker/daemon.json`文件，将选中的镜像地址添加到`registry-mirrors`数组里（可同时填入多个镜像）：  

    
    {
        "registry-mirrors": [
            "https://registry.docker-cn.com"
        ]
    }

此处的`registry.docker-cn.com`是 Docker 的官方中国镜像， 除此之外还有其他一些第三方镜像可选：  

| 镜像        | 地址                               |
|-------------|------------------------------------|
| Azure  中国 | https://dockerhub.azk8s.cn         |
| 中科大      | https://docker.mirrors.ustc.edu.cn |
| 七牛云      | https://reg-mirror.qiniu.com       |
| 网易云      | https://hub-mirror.c.163.com       |
| 腾讯云      | https://mirror.ccs.tencentyun.com  |

保存文件之后重启一下 Docker 服务：

    sudo systemctl daemon-reload
    sudo systemctl restart docker

不出意外的话，接下来就能享受到国内镜像的丝滑快速了。  

## 镜像的搜索与拉取

Docker 安装并配置成功后，就可以通过 Docker 安装镜像了。  

### 镜像搜索

可以通过如下命令搜索所需的镜像：  
    
    $ docker search [name]

比如，搜索与`go`相关的镜像：  
    
    $ docker search go
    NAME                                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
    golang                                    Go (golang) is a general purpose, higher-lev…   3342                [OK]                
    gogs/gogs                                 Gogs is a painless self-hosted Git service.     635                                     [OK]
    google/cadvisor                           DEPRECATED: New images will NOT be pushed.  …   418                                     
    google/cloud-sdk                          Google Cloud SDKbundle with all components a…   331                                     [OK]
    goofball222/unifi                         Ubiquiti Networks UniFi Controller software …   126                                     [OK]
    ipfs/go-ipfs                              Go implementation of IPFS, the InterPlanetar…   87                                      [OK]
    gocd/gocd-server                          GoCD server running on docker                   77                                      
    gogs/gogs-rpi                             Gogs Docker image for Raspberry Pi 2            55                                      
    serjs/go-socks5-proxy                     go-socks5-proxy                                 50                                      [OK]
    allinurl/goaccess                         Official build of GoAccess                      45                                      [OK]
    ginuerzh/gost                             GO Simple Tunnel - a simple tunnel written i…   31                                      [OK]
    gobuffalo/buffalo                         http://gobuffalo.io                             16                                      [OK]
    circleci/golang                           CircleCI images for Go                          15                                      [OK]
    appleboy/gorush                           A push notification server written in Go (Go…   11                                      [OK]
    hairyhenderson/gomplate                   A commandline Golang template processor         8                                       [OK]
    trivago/gollum                            Gollum docker image                             5                                       [OK]
    governmentpaas/s3-resource                S3 resource with IAM profile support            5                                       [OK]
    atlassianlabs/gostatsd                    An implementation of Etsy's statsd in Go. ht…   4                                       
    byzantinelab/go-tangerine                 Tangerine netowrk go client                     1                                       
    govuk/government-frontend                 Public-facing app to display documents on ww…   1                                       [OK]
    governmentpaas/semver-resource            Concourse semver resource with IAM profile s…   1                                       [OK]
    jetbrainsinfra/golang                     Golang + custom build tools                     1                                       [OK]
    webhippie/golang                          Docker images for Golang                        1                                       [OK]
    a3linux/go-aws-mon                        go-aws-mon                                      0                                       
    jrandall/gocd-agent-ubuntu-18.04-docker   gocd-agent-ubuntu-18.04-docker                  0                                       [OK]

可以看到，搜索结果列表中的第一项为 Go 的官方镜像，由`OFFICIAL`一列进行标识。  

除此以外还有一些非官方的镜像，可以通过查看其对应的描述了解其与官方镜像的差别。  

### 镜像拉取

找到想要拉取安装的镜像后，即可通过如下命令进行拉取：  
    
    $ docker pull [name]

比如，下载并安装`hello-world`镜像：

    $ docker pull hello-world 
    Using default tag: latest
    latest: Pulling from library/hello-world
    0e03bdcc26d7: Pull complete
    Digest: sha256:4cf9c47f86df71d48364001ede3a4fcd85ae80ce02ebad74156906caff5378bc
    Status: Downloaded newer image for hello-world:latest
    docker.io/library/hello-world:latest


可以看到，只输入镜像名时，默认拉取的标签为`latest`。  

通过如下方式可以拉取指定标签：  
    
    $ docker pull [name]:[tag]

查看当前已安装的 Docker 镜像列表：  
    
    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    hello-world         latest              bf756fb1ae65        8 months ago        13.3kB

## 运行镜像创建容器

镜像拉取成功后，可以通过如下命令运行：  

    $ docker run hello-world
    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
     1. The Docker client contacted the Docker daemon.
     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
     3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
     4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
     $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
     https://hub.docker.com/

    For more examples and ideas, visit:
     https://docs.docker.com/get-started/

每个容器的使用方法都不完全一样，请在使用前查看文档。     

## 镜像与容器管理

可以使用如下命令查看正在运行的容器的状态： 

    $ docker ps

查看正在运行的容器及其当前状态：
    
    $ docker container ls

查看当前所运行镜像的 CPU、 RAM以及网络使用情况：  
    
    $ docker stats

查看 Docker 的网络配置情况：  
    
    $ docker network ls

停止并移除容器：  
    
    $ docker stop [name]
    $ docker rm [name]


