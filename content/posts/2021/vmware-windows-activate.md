---
title: "BLWL[38] 激活 vmware 中的 Windows 操作系统"
date: 2021-04-27T14:19:09+08:00
categories: ["Better Life With Linux"]
tags: ["vmware", "windows"]
draft: false
---

为了方便平时工作（主要是为了用 visio 做图），之前在 vmware 中配置了一个 Windows 10 的虚拟机。  

一般的在物理机上适用的激活方式尝试后没能成功在虚拟机中激活 Windows，后来发现了如下方式，在此记录一下。  


首先以管理员身份运行 cmd，然后以此输入以下命令：  

```
slmgr.vbs /upk
slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX
slmgr /skms zh.us.to
slmgr /ato
```

其中第二条命令的编码无需修改。  

不出意外的话，系统已成功激活。  
