---
title: "[论文阅读笔记 -- 跨模态检索] Multi-Modality Cross Attention Network (CVPR 2020)"
date: 2021-07-14T10:42:26+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "retrieval", "cross-modal", "attention"]
draft: false
---

# Multi-Modality Cross Attention Network for Image and Sentence Matching (CVPR 2020)

![Fig 1](/images/2021/PRN45/1.png)

## 出发点

既考虑模态间关联，也考虑模态内关联。  

提出多模态交叉注意力网络 (Multi-Modality Cross Attention Network)，主要由自注意力模块与交叉注意力模块组成。  

![Fig 2](/images/2021/PRN45/2.png)

## 实例候选提取 (Instance Candidate Extraction)

### 视觉
bottom-up attention model pretrained on Visual Genome + FC  

### 文本
Word-Piece tokens  

![Fig 3](/images/2021/PRN45/3.png)

## 自注意力模块

用于建模模态间关联。  

注意力用 Transformer 实现。  

## 交叉注意力模块

拼接后用 Transformer。  

## 目标函数

三元组损失。  
