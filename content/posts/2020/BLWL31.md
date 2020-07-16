---
title: "BLWL[31] GitHub下fork后同步源的新更新内容"
date: 2020-07-16T14:24:33+08:00
categories: ["Better Life With Linux"]
tags: ["git", "Linux", "github", "fork"]
draft: false
---

在GitHub下，当A开发者fork了B开发者的库后，若B开发者更新了内容，A开发者可以通过如下方法实现库同步。  

## 给fork配置远程库

### 查看远程状态

```
$> git remote -v
```

### 添加将被同步给 fork 远程的上游仓库

```
$> git remote add upstream (upstream_url)
```

### 再次查看状态确认是否配置成功

## 同步fork

### 从上游仓库 fetch 分支和提交点，提交给本地 master，并被存储在一个本地分支 upstream/master

```
$> git fetch upstream
```

### 切换到本地主分支(如果不在的话)

```
$> git checkout master
```

### 把 upstream/master 分支合并到本地 master 上，这样就完成了同步，并且不会丢掉本地修改的内容

```
$> git merge upstream/master
```

### 如果想更新到 GitHub 的 fork 上，则

```
$> git push origin master
```
