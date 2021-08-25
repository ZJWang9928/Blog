---
title: "炼丹杂记 -- 枢纽点问题 (Hubness Problem)"
date: 2021-08-25T18:08:27+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "hubness"]
draft: false
---

当特征嵌入空间是一个高维空间时，容易出现枢纽点问题 (Hubness Problem)。  

### 问题定义

在高维空间中，一部分测试集的类别可能会成为很多数据点的 K 近邻 (KNN)，但其类别之间却没什么关系。在使用语义空间 (semantic space) 作为嵌入空间时，需要将视觉特征映射到语义空间中，这样会使得空间发生萎缩，点与点之间更加稠密，从而加重 hubness problem。  
