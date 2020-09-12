---
title: "BLWL[29] git拉取库时断点续传的实现"
date: 2020-06-19T20:24:14+08:00
categories: ["Better Life With Linux"]
tags: ["git", "Linux", "git clone", "git fetch"]
draft: false
---

git clone 不支持断点续传，拉取失败的库总是需要从头开始拉取，库大网慢的时候实在是很心累。    

可以用 git fetch 解决这个问题。     


    # 建立同名目录并进入
    $> mkdir dirname
    $> cd dirname

    # 初始化并开始拉取
    $> git init
    $> git fetch GIT_REPO_URL

    # 中断则重复 fetch 即可继续
    $> git fetch GIT_REPO_URL

成功后出现如下字样      

    来自 GIT_REPO_URL
     * branch            HEAD       -> FETCH_HEAD

意味着已成功将最新数据 fetch 到了本地的 FETCH\_HEAD 分支上去。        
通过如下命令切换分支即可。  
    
    $> git branch -a
    $> git checkout FETCH_HEAD
