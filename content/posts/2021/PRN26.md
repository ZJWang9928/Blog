---
title: "[论文阅读笔记 -- 频域 / 图网络] Beyond Low-frequency Information in GCNs (2021)"
date: 2021-06-29T12:00:44+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "frequency", "graph"]
draft: false
---

# 2101.00797 Beyond Low-frequency Information in Graph Convolutional Networks

## 背景

当前一些研究认为平滑信号，即低频信息是 GNN 成功的关键。  

本文重点思考是否低频信息就能完全满足需求，以及其他信息在 GNN 中扮演着怎样的角色。  

### 低通滤波器的两个问题

+ GNN 中的低通滤波器主要用于保留节点特征共性，其不可避免地忽视了差异。由于低频信息的平滑性，这一机制在同配网络 (assortative networks) 即相似节点倾向于互相联结的网络上运行良好，但是在来自不同类的节点互相联结的异配网络 (disassortative networks) 上这样的机制会影响表现。在这种情况下，捕捉节点之间差异的高频信息，甚至同时包含高低频信息的原始特征，都会更加合适。  

+ 总是使用低通滤波器会导致节点特征变得难以区分，引起过平滑 (over-smoothing)。  

实验发现高低频信号对节点特征的学习都有帮助，而当网络呈现异配性时，高频信号的表现比低频信号更好。  

由此，需要探讨的问题转化为了该怎样使用 GNN 中不同频率的信号，并使 GNN 适用于不同类型的网络。  

### 回答上述问题需要解决的两个困难
+ 传统滤波器难以同时很好地提取不同频率的信号，都是为某一特定频率设计的
+ 即使能够提取不同信息，不同应用场景中的网络结构差异很大，任务与信息之间的关系复杂，很难决定该使用哪种频率信号

本文设计了一种通用的频率自适应图卷积网络 (general frequency adaptation graph convolutional network, FAGCN)，自适应地聚合来自邻居节点或自身的不同信号。

![Fig 1](/images/2021/PRN26/1.png)

## 实验分析

在节点分类 (node classification) 任务上，逐渐提高网络的异配性以观察低频与高频信号表现的变化。  

为此构建一个包含 200 个节点的网络，将这些节点随机分成两类，对两类中的各节点分别从高斯分布 \\(\mathcal{N}(0.5, 1)\\) 和 \\(\mathcal{N}(-0.5, 1)\\) 采样 20-D 的特征向量，同类之间的联结以 \\(p = 0.05\\) 的概率从伯努利分布中采样，异类之间联结的概率 \\(q\\) 在 0.01 到 0.1 之间变化，随着 \\(q\\) 逐渐增大，网络从同配逐渐变得异配。  

现有 GNN 在 \\(q\\) 增大后表现变差就是因为其只从邻居节点聚合低频信号，使得节点特征变得相似，却不管这些节点是否是同类节点。FAGCN 结合高低频信号，因而能够得到最好的表现。  

## 图傅利叶变换

根据图信号处理 (graph signal processing) 理论，可以将归一化拉普拉斯矩阵的特征向量用作图傅利叶变换的基。对于一个信号 \\(x \in \mathbb{R}^n\\)，图傅利叶变换定义为 \\(\hat{x} = U^Tx\\)，而逆图傅利叶变换为 \\(x = U\hat{x}\\)。因此信号 \\(x\\) 与卷积核 \\(f\\) 之间的卷积 \\(\*\_{G}\\) 定义为：  

$$f \*\_{G} x = U((U^Tf) \odot (U^Tx)) = Ug_{\theta}U^Tx,$$  

其中 \\(\odot\\) 是哈达玛积，\\(g_{\theta}\\) 替换了 \\(U^Tf\\)，是一个对角矩阵，表示频域中的卷积核。GCN 中卷积核定义为 \\(g_{\theta} = I - \Lambda\\)。  

## 信号分离

![Fig 2](/images/2021/PRN26/2.png)

设计了低通滤波器 \\(\mathcal{F}\_{L}\\) 和高通滤波器 \\(\mathcal{F}\_{H})\\) 从节点特征分离高低频信号：  

$$F_{L} = \epsilon I + D^{-1/2}AD^{-1/2} = (\epsilon + 1)I - L,$$
$$F_{H} = \epsilon I - D^{-1/2}AD^{-1/2} = (\epsilon - 1)I - L,$$

\\(\epsilon \in [0, 1]\\) 是放缩超参数。用两个滤波器取代上一节式子中的 \\(f\\)：  

$$\mathcal{F}\_{L} \*\_{G} s = U[(\epsilon + 1)I - \Lambda]U^Tx = \mathcal{F}\_{L} \cdot x,$$
$$\mathcal{F}\_{H} \*\_{G} s = U[(\epsilon - 1)I - \Lambda]U^Tx = \mathcal{F}\_{H} \cdot x.$$

因而，\\(\mathcal{F}\_{L}\\) 的卷积核 \\(g_{\theta} = (\epsilon + 1)I - \Lambda\\)，可改写为 \\(g_{\theta}(\lambda_{i}) = \epsilon + 1 - \lambda_{i}\\)。为避免出现负值，取带二阶卷积核 \\(g_{theta}(\lambda_{i}) = (\epsilon + 1 - \lambda_{i})^2\\) 的滤波器 \\(\mathcal{F}\_{L}^2\\)，由图 2 可以看出，\\(\lambda_{i} = 0\\) 时 \\(g_{\theta} = (\epsilon + 1)^2 > 1\\)，\\(\lambda_{i} = 2\\) 时 \\(g_{\theta} = (\epsilon - 1)^2 < 1\\)，其放大了低频信号并制约了高频信号。  

与传统的低通滤波器相比，如二阶 GCN 的卷积核为 \\(g_{\theta} = (1 - \lambda_{i})^2\\)，\\(\lambda_{i} = 0\\) 时 \\(g_{\theta} = 1 < (\epsilon + 1)^2\\)，故此 \\(\mathcal{F}\_{L}\\) 比 GCN 的低通滤波器为低频信号提供了更大的值，是增强的滤波器 (enhanced filters)。\\(\mathcal{F}\_{H}\\) 同样也为高频信号提供了更大的值。  

### 从节点分离高低频信号的两个问题
+ 选择信号需要先验知识
+ 涉及矩阵乘法，不适合较大的图

因而，还需要一种能够自适应聚合高低频信号的高效方法。  

### 高低频信号的空域意义
由两个滤波器的定义可以看出，其差异就是加减不同，故此  
+ 低频信号 \\(\mathcal{F}\_{L} \cdot x\\) 是空域中节点特征及其邻居节点特征的和
+ 高频信号 \\(\mathcal{F}\_{H} \cdot x\\) 是空域中节点特征与其邻居节点特征之间的差异

## 信号聚合

![Fig 3](/images/2021/PRN26/3.png)

模型输入为节点特征 \\(H = \\{h_{1}, \cdots, \h_{N}\\} \in \mathbb^{N \times F}\\)。引入注意力机制学习高低频的比重：  

$$\widetilde{h}\_{i} = \alpha_{ij}^{L}(\mathcal{F}\_{L} \cdot H)_{i} + \alpha_{ij}^{H}(\mathcal{F}\_{H} \cdot H)_{i} = \epsilon h\_{i} + \sum\_{j \in \mathcal{N}\_{i}} \frac{\alpha\_{ij}^L - \alpha\_{ij}^H}{\sqrt{d\_{i}d\_{j}}} h\_{j},$$

其中 \\(\widetilde{h}\_{i}\\) 为聚合后的节点 \\(i\\)，\\(\mathcal{N}\_{i}\\) 和 \\(d\_{i}\\) 分别表示邻居节点集合与节点 \\(i\\) 的度。令 \\(\alpha\_{ij}^L + \alpha\_{ij}^H = 1\\) 且 \\(\alpha\_{ij}^G = \alpha\_{ij}^L - \alpha\_{ij}^H\\)。  

可以从两个角度解释 \\(\alpha\_{ij}^G\\)：  

+ \\(\alpha\_{ij}^G\\) 的符号间接表征了高低频信号比重
+ \\(\alpha\_{ij}^G\\) 表示聚合中邻居节点的系数，为正表示表示节点特征与邻居节点求和，为负表示其间差异，为零时邻居节点特征贡献有限，此时原始特征 (raw features) 占据节点特征表达的主导  

为了有效学习 \\(\alpha\_{ij}^G\\)，需要同时考虑节点本身及其邻居的特征，本文设计了一种共享的自门控机制 (shared self-gating mechanism) \\(\mathbb{R}^F \times \mathbb{R}^F \rightarrow \mathbb{R}\\)：  

$$\alpha\_{ij}^G = tanh(g^T[h\_{i} || h\_{j}]),$$  

其中 \\(||\\) 表示拼接操作，\\(g \in \mathbb{R}^{2F}\\) 是共享的卷积核。为了利用结构信息，只在节点及其一阶邻居节点 \\(\mathcal{N}\_{i}\\) 之间计算系数。  

得到系数后，可以如下聚合邻居节点：  

$$h\_{i}^{'} = \epsilon h\_{i} + \sum\_{j \in \mathcal{N}\_{i}} \frac{\alpha\_{ij}^G}{\sqrt{d\_{i}d\_{j}}} h\_{j}.$$

度数用于对系数进行归一化，避免聚合后的特征过大。  

## FAGCN 的整体架构

先由 MLP 对原始特征进行非线性变换，\\(\phi\\) 表示激活函数。   

$$h\_{i}^{(0)} = \phi(W\_{1}h\_{i}) \qquad \in \mathbb{R}^{F' \times 1}$$
$$h\_{i}^{(l)} = \epsilon h\_{i}^{(0)} + \sum\_{j \in \mathcal{N}\_{i}} \frac{\alpha\_{ij}^G}{\sqrt{d\_{i}d\_{j}}} h\_{j}^{(l-1)} \qquad \in \mathbb{R}^{F' \times 1}$$
$$h\_{out} = W\_{2}h\_{i}^{(L)} \qquad \in \mathbb{R}^{K \times 1}$$

## FAGCN 与现有 GNN 的联系

+ FAGCN 是大多数现有 GNN 的一般化
+ 令 \\(\alpha\_{ij}^G = 1\\) 则 FAGCN 表现得像 GCN
+ 使用 softmax 归一化 \\(\alpha\_{ij}^G = 1\\) 则变成 GAT
+ 由于 GCN 与 GAT 中系数均大于 0，它们都倾向于聚合低频信号；而 FAGCN 能够学习一个可正可负的系数，自适应聚合高低频信号

## FAGCN 的表达能力 (Expressive Power)

从节点特征之间距离的角度分析 FAGCN 的表达能力，一对相连节点 \\((u, v)\\) 的特征 \\((h\_{u}, h\_{v})\\) 之间计算节点特征之间距离 \\(\mathcal{D}\\)、节点特征低频信号之间距离 \\(\mathcal{D}\_{L}\\) 以及节点特征高频信号之间距离 \\(\mathcal{D}\_{H}\\) 如下：  

$$\mathcal{D} = ||h\_{u} - h\_{v}||\_{2},$$
$$\mathcal{D}\_{L} = ||(\epsilon h\_{u} + h\_{v}) - (\epsilon h\_{v} + h\_{u})||\_{2} = |1 - \epsilon| \mathcal{D},$$
$$\mathcal{D}\_{H} = ||(\epsilon h\_{u} - h\_{v}) - (\epsilon h\_{v} - h\_{u})||\_{2} = |1 + \epsilon| \mathcal{D}.$$

### 命题 1
低通滤波使特征变得相似，而高通滤波使得特征变得有鉴别力。  

#### 简证
可以看到 \\(\mathcal{D}\_{L} < \mathcal{D} < \mathcal{D}\_{H}\\)。这表明由低频信号求得的距离比原始信号更小，而高频信号引入的距离比原始距离更大。  

### 命题 2
大多数现有的 GNN (如 GCN) 只有使节点特征变得相似的能力。  

## 缓解过平滑问题

![Fig 4](/images/2021/PRN26/4.png)

通过对比在不同模型深度下 GCN 与 FAGCN 的表现进行分析。可以看出 GCN 表现随着层数增加而急剧下跌，这表明 GCN 严重受到过平滑问题的困扰。FAGCN 表现更稳定而且更高，主要有两个原因：  

+ 负值权重可以防止节点特征太过相似
+ 在各层加入包含高低频信息的原始特征，进一步防止节点特征变得难以区分

## 边系数可视化

![Fig 5](/images/2021/PRN26/5.png)
