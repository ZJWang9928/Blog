---
title: "BLWL[22] 撤销git提交"
date: 2020-01-23T22:44:58+08:00
categories: ["Better Life With Linux"]
tags: ["Git"]
draft: false
---

# Git Commit Withdrawal

> 撤销上一次提交  
> git reset --soft HEAD^   
> 撤销前n次提交    
> git reset --soft HEAD~n   
> 仅修改 commit 注释    
> git commit --amend    


### 参数：
+ --soft 不删除工作空间改动代码，撤销 commit，不撤销 git add . 
+ --hard 删除工作空间改动代码，撤销 commit，撤销 git add . 
+ --mixed 不删除工作空间改动代码，撤销 commit，并且撤销 git add . 操作，默认参数，与 git reset --mixed HEAD^ 和 git reset HEAD^ 效果一样
