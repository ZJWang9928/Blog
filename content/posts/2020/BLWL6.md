---
title: "BLWL[6] Manjaro KDE安装Google拼音输入法"
date: 2020-01-23T22:28:58+08:00
categories: ["Better Life With Linux"]
tags: ["Manjaro", "Linux"]
draft: false
---
先安装相关软件  

	sudo pacman -S fcitx-im fcitx-configtool fcitx-googlepinyin

然后在用户根目录编辑 .xprofile 文件（没有就新建一个）

	vim ~/.xprofile

若是刚装好的 Manjaro 上使用 vim 需要安装一下， pacman 即可  
内容写：

	export LC_ALL=zh_CN.UTF-8
	export GTK_IM_MODULE=fcitx
	export QT_IM_MODULE=fcitx
	export XMODIFIERS="@im=fcitx"

保存，重启（或注销再登录），在输入法里面选择 Configure Current Input Method 即可配置快捷键  
