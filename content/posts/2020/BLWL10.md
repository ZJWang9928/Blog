---
title: "BLWL[10] Manjaro更新时Invalid Software Package问题解决方案"
date: 2020-01-23T22:32:58+08:00
categories: ["Better Life With Linux"]
tags: ["Manjaro", "Linux"]
draft: false
---
Manjaro更新报错-无效或已损坏的软件包(PGP 签名)    

原因：使用了社区源且开启了验证，关闭验证即可  
  
        vim /etc/pacman.conf

找到社区源相关部分   

        [archlinuxcn]
        #SigLevel = Optional TrustedOnly
        SigLevel = Optional TrustAll
        Server = http://repo.archlinuxcn.org/$arch
