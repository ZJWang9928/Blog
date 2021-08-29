---
title: "[论文阅读笔记 -- ReID] Learn 3D Shape Feature for Texture-insensitive Person ReID (CVPR 2021)"
date: 2021-08-29T14:30:30+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "3D", "reid", "retrieval"]
draft: false
---

# Learning 3D Shape Feature for Texture-insensitive Person Re-identification (CVPR 2021)

[开源代码传送门](https://github.com/TencentYoutuResearch/PersonReID-YouReID)

![Fig 1](/images/2021/PRN86/1.png)

## 背景

Person ReID 任务。  

研究表明 Person ReID 相当依赖衣着外观纹理 (clothing appearance textures)，大多数现有方法在衣着纹理较迷惑时表现大幅下降。由于人们可能会换衣服，且不同的人可能会穿相似的衣服，因而衣着纹理对 ReID 而言并不可靠。  

本文对更具有鉴别能力的特征进行建模，即人类形状表征 (human shape representation)。  

### 现有方法学习形状相关特征的两种方法

+ 2D 图像空间。基于 2D 的方法主要尝试基于如等高线、关键点等视觉统计量来提取形状特征，或通过 adversarial feature disentangle，这些方法只利用了 2D 空间中的结构和形状信息，而深度以及 3D 相对位置等 3D 信息被忽略。
+ 3D 源数据。基于 3D 的源数据需要通过外部设备来获取，在监控摄像环境中可能难以实现。

本文通过将 3D 重构作为 ReID 特征学习的辅助任务与正则化，直接从原始的 2D 图像中提取对纹理不敏感的 3D 特征。采用一种多任务框架 (multi-task framework)，ReID 特征受到识别 (identification) 损失 (即 softmax loss + triplet loss)，以及 3D 重构损失的监督。  

训练 3D 行人重构的一大障碍在于 3D ground truth 的缺失，因而本文设计了一种纯无监督的架构，称为对抗自监督投影 (Adversarial Self-Supervised Projection, ASSP)。首先使用外部的无标签 3D 数据训练一个鉴别器，用于区分真实的 3D 参数与重构而来的结果。进而引入一个自监督学习循环，将 3D 重构结果投影回 2D 平面，最小化其间差异。  

3D 人类重构 (3D human reconstruction) 倾向于得到一个形状平均的表征 (mean shape representation)，因而全局的 3D 形状特征并不具备足够的鉴别力。本文提出一种多粒度形状特征学习 (Multi-Granularity Shape feature learning, MGS)，结合全局与局部形状特征来提升 ReID 的鉴别能力。  

![Fig 2](/images/2021/PRN86/2.png)

## 3D Shape Learning (3DSL)

### 3D Parametric Model

本文选用 SMPL 作为基本模型进行重构，其建模为 pose 参数 \\(\theta \in \mathbb{R}^{24 \times 3}\\) 和 shape 参数 \\(\beta \in \mathbb{R}^{10}\\) 的函数，返回 \\(N_{V} = 6890\\) 个顶点以及 \\(N_{F} = 13776\\) 张脸。  

但是 shape 参数只有 \\(10\\) 维，表征容量不够，因而本文引入 vertice-wise displacement values，记为 \\(\delta \in \mathbb{R}^{6890 \times 3}\\)：  

$$\mathcal{M}(\beta, \theta, \delta) = W(T(\beta, \theta, \delta), J(\beta), \theta, \mathbb{W}),$$

其中 \\(W\\) 是作用于 rest pose \\(T(\beta, \theta, \delta)\\) 以及 skeleton joints \\(J(\beta)\\) 的 linear blend-skinning 函数。  

将 3D ReID 形状特征记为 \\(F_{shape}\\)，由此去得到 \\(\theta\\) 和 \\(\delta\\)。  

### 3D 形状特征提取

首先由 \\(E_{3D}\\) 模型提取包含所有 3D 信息的特征，其输出传入 3D 重构网络。  

3D 模型的参数分为两组：  

+ 与形状无关的参数：姿态参数 \\(\theta\\) 和相机参数 \\(\psi\\)
+ 与形状相关的参数：形状参数 \\(\beta\\) 和 vertice-wise displacement \\(\delta\\)

采用独立的子网络 \\(E_{pose}\\) 和 \\(E_{shape}\\) 为两组参数分解信息。  

### 对抗自监督投影 (ASSP)

#### 3D 重构对抗学习

训练一个鉴别器。  

通过 Rodrigues formula 将各 jonit 的 \\(3\\) 维旋转向量变换为 \\(3 \times 3\\) 的旋转矩阵，即将 \\(\theta\\) 从 \\(24 \times 3\\) 变换到 \\(24 \times 3 \times 3\\)。  

将变换后的姿态参数以及 \\(\beta\\) 作为鉴别器 \\(D\\) 的输入，一个大规模的 SMPL 参数数据集被用作真实的人类身体数据，记为 \\(\theta_{real}\\) 和 \\(\beta_{real}\\)，\\(D\\) 的对抗损失为  

$$\mathcal{L}\_{adv}(D) = \mathbb{E}[(1 - D(\theta_{real}, \beta_{real}))^{2}] + \mathbb{E}[D(\theta, \beta)^{2}],$$

用于训练 \\(E_{3D}\\)、\\(E_{pose}\\) 以及 \\(E_{shape}\\) 的对抗损失为

$$\mathcal{L}\_{adv}(E_{3D}, E_{pose}, E_{shape}) = \mathbb{E}[(1 - D(\theta, \beta))^{2}].$$

#### 从 3D 到 2D 的自监督投影

本文选用关键点 (keypoints) 和轮廓 (silhouettes) 作为中介来联接 2D 和 3D 空间。  

用 off-the-shelf 的检测器从原始图像预测关键点位置 \\(K \in \mathbb{R}^{P \times 2}\\)，用 GrabCut 预测轮廓 \\(M\\)，由于投影涉及相机的 3D 位置，同时也预测了相机位置参数 \\(\psi \in \mathbb{R}^{3}\\)。  

关键点的投影通过由 \\(\psi\\) 和相机模型得来的投影矩阵做稀疏映射 (sparse mapping) 得到，关键点投影损失为  

$$\hat{K} = \Pi_{key}(\beta, \theta, \delta, \psi).$$
$$\mathcal{L}\_{key} = ||K - \hat{K}||^{2}\_{2},$$

轮廓投影借助一个可微的 renderer，本文选用 neural renderer，使其能够端到端训练，轮廓投影损失为

$$\hat{M} = \Pi_{sil}(\beta, \theta, \delta, \psi),$$
$$\mathcal{L}\_{sil} = ||M - \hat{M}||^{2}\_{2} + ||\delta||\_{2},$$

限制 \\(\delta\\) 的值以避免覆盖衣着细节，并保持重构得到的 mesh 平滑。  

### 多粒度形状特征 (MGS)

MGS 特征被用作子网络的输入，以预测各身体部分的 vertice-wise displacement，促使该特征包含局部形状信息。  

具体如图 3 所示。  

![Fig 3](/images/2021/PRN86/3.png)

## 对纹理不敏感的 RGB 特征

\\(E_{rgb}\\) 分支用于挖掘其他重要的特征。  

自适应地调整了三元组损失的采样策略：  

+ 对于衣着有变化的数据集，将衣着不同的同一行人选为正样本对
+ 对于不同行人衣着相似的情况，将其作为负样本对
+ 处理一般的 ReID 问题时，正负样本对随机选取用于训练 \\(E_{rgb}\\)
