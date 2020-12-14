---
title: "BLWL[36] Ubuntu 安装显卡驱动时 No additional drivers available 问题解决方案"
date: 2020-12-14T23:50:32+08:00
categories: ["Better Life With Linux"]
tags: ["Ubuntu", "drivers", "Nvidia", "Linux", "cv"]
draft: false
---

新配置的 Ubuntu 炼丹工作站在通过 Software and Updates 中的 Additional Drivers 选项卡安装 GPU 显卡驱动时出现了 No additional drivers available 的问题。  

解决方案如下。  

```
sudo add-apt-repository ppa:xorg-edgers/ppa
sudo apt-get update
```

完成后回到 Software and Updates 中的 Additional Drivers 选项卡，不出意外的话就能看见可选项了。  
