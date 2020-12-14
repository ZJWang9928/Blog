---
title: "BLWL[35] Ubuntu 上向日葵被连接时闪退问题解决方案"
date: 2020-12-14T23:18:03+08:00
categories: ["Better Life With Linux"]
tags: ["Ubuntu", "sunlogin", "Linux", "cv"]
draft: false
---

新配置的 Ubuntu 炼丹工作站上装的向日葵一被连接就闪退，查阅了一些资料后顺利解决了这个问题，具体方法如下。  

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install lightdm
```

安装 lightdm 的过程中会让选择使用原始桌面还是 lightdm，此时选择 lightdm，完成安装后重启工作站即可。  
