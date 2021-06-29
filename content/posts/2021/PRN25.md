---
title: "[论文阅读笔记 -- 频域 / 图网络] Spectral Graph Attention Network (2020)"
date: 2021-06-29T10:34:36+08:00
categories: ["Paper Reading Notes"]
tags: ["paper reading", "cv", "notes", "cross-modal", "reid"]
draft: false
---

# 2003.07450 Spectral Graph Attention Network (2020)

![Fig 1](/images/2021/PRN25/1.png)

## 背景

图注意力网络 (Graph Attention Network, GAT) 通过引入注意力机制优化 GCN 的卷积过程。具体来说，在节点聚合 (node aggregation) 阶段，GAT 赋予各边一个自注意力权重，用于捕捉与邻居节点之间的局部相似度。    

GAT 及其相关变体以一种直接的方式考虑注意力，即在空域中学习边注意力。在这种情况下，这类注意力能够捕捉到图的局部结构，即来自邻居节点的信息，但其无法显式编码图的全局结构。此外为每条边计算注意力权重很低效，尤其是图较大的时候。  

在计算机视觉中，一张自然图像可以被分解为低频成分和高频成分。低频成分包含变化平滑的结构，如背景部分；而高频成分描述变化较快的细节，如轮廓。  

在图表征学习 (graph representation learning) 中，由于图信号处理 (graph signal processing, GSP) 提供了一种基于图中拉普拉斯矩阵升序特征值 (ascending ordered eigenvalues of Laplacian in graphs) 的方法，可以直接分解高低频成分，这一分解可以被更自然地观察到。  

小的特征值对应的特征向量包含平滑变化的信号，使得邻居节点共享相似的值。相反，大的特征值对应的特征向量包含沿着边剧烈变化的信号。只用低频成分构建时，图倾向于保留簇 (cluster) 内信息；而只用高频成分构建时，图倾向于保留簇之间的信息。频域中的低频与高频成分可能正相应反应了图在空域中的局部与全局结构信息。  

本文设计了一种频谱图注意力网络 (Spectral Graph Attention Network, SpGAT)，将注意力机制拓展到图的频域空间，从全局视角显式编码图的结构信息。  

SpGAT 中选择图小波 (graph wavelets) 作为频谱的基 (spectral bases) 根据其索引分解为低频与高频成分。接着根据低频与高频成分分别构建两个卷积核，在卷积核上引入注意力机制捕捉其相应的重要性。最后用池化函数与激活函数产生输入。  
进而使用切比雪夫多项式逼近 (Chebyshev Polynomial approximation) 计算图的谱小波 (spectral wavelets of graphs)，并提出在大型图上更加高效的变体模型 SpGAT-Cheby。  

![Fig 2](/images/2021/PRN25/2.png)

除了理解为空域上的邻居节点聚合，原始的 GCN 也可以被看作是在频域上的图信号处理 (GSP)：  

$$g_{\theta} \star x = Bg_{\theta}B^Tx,$$  

其中 \\(x\\) 是每个节点上的信号，\\(B = \\{b_{1}, \cdots, b_{n}\\}\\) 是从图中提取的频谱基 (spectral bases)，\\(g_{\theta} = diag(\theta)\\) 是以 \\(\theta\\) 为参数的对角过滤器 (diagonal filter)。  

基于上式，GCN 可以视为基于在一阶切比雪夫多项式逼近图上的傅利叶变换的谱图卷积 (spectral graph convolution based on the Fourier transform on graphs with first-order Chebyshev polynomial approximations)。  

谱图卷积进一步分为两个阶段：  

$$feature \ transformation: \ X = H\Theta^T,$$
$$graph \ convolution: \ H' = \sigma(BFB^TX),$$  

其中 \\(F\\) 是作为图卷积核的对角矩阵。在这个例子中 \\(F = diag(\lambda_{1}, \cdots, \lambda_{n})\\)，\\(\\{\lambda_{i}\\}_{i=1}^n\\) 是规范化图拉普拉斯矩阵 \\(L = I - \hat{A}\\) 的升序特征值，频谱基 \\(B\\) 为相应的特征向量。  

## SpGAT 层的构建

从 GSP 视角，\\(F\\) 中的对角化值 \\((f_{1}, \cdots, f_{n})\\) 可以被看作图上的频率，将这些值根据其索引的小/大标记为低/高频。同时，\\(B\\) 中相应的频谱基即为低/高频成分。  

首先将频谱基分成两组并改写上述两阶段公式：  

$$feature \ transformation: \ X_{L} = H\Theta_{L}^T, \ X_{H} = H\Theta_{H}^T,$$
$$graph \ convolution: \ H' = \sigma(AGG(B_{L}F_{L}B_{L}^TX_{L}, B_{H}F_{H}B_{H}^TX_{H})),$$  

其中 \\(B_{L} = \\{b_{1}, \cdots, b_{d}\\}, B_{H} = \\{b_{d+1}, \cdots, b_{n}\\}\\) 分别为低/高频成分。式中的 \\(F_{L}\\) 和 \\(F_{H}\\) 可以被视为低/高频的重要性，因而本文通过采用再参数化技巧 (re-parameterization trick) 进一步引入可学习的注意力权重：  

$$H' = \sigma(AGG(B_{L}\alpha_{L}B_{L}^TX_{L}, B_{H}\alpha_{H}B_{H}^TX_{H})),$$  

式中 \\(\alpha_{\*} = diag(\alpha_{\*}, \cdots, \alpha_{\*})\\) 是两个由可学习注意力 \\(\alpha_{L}\\) 和 \\(\alpha_{H}\\) 参数化的对角矩阵 (等号左侧是矩阵，右侧是标量，写到这里发现有必要涉及符号加粗但是懒得改了所以标注一下)。为确保 \\(\alpha_{\*}\\) 为正且可以比较，用 softmax 对齐进行归一化。  

理论上有很多再参数化的方式，如关于频谱基的自注意力等。但是这类方法无法反应高低频成分的本质，而且会引入过多额外需要学习的参数。  

## 频谱基的选取

频谱基的选取是 SpGAT 另一个重要问题。本文选择图小波作为频谱基，而非较为常见的傅里叶基。  

谱图小波 \\(\psi_{si}(\lambda)\\) 定义为以节点 \\(i\\) 为中心的信号 \\(x\\) 在频域中调制产生的信号。对于一张图 \\(G\\)，图小波变换使用一组小波 \\(\Psi_{s} = (\psi_{s1}(\lambda_{1}), \psi_{s2}(\lambda_{2}), \cdots, \psi_{sn}(\lambda_{n}))\\) 作为基，谱图小波变换定义如下：  

$$\Psi_{s}(\lambda) = Ug_{s}(\lambda)U^T,$$  

其中 \\(U\\) 为归一化图拉普拉斯矩阵 \\(L = I - \hat{A}\\) 的特征向量，\\(g_{s}(\lambda) = diag(g_{s}(\lambda_{1}), \cdots, g_{s}(\lambda_{n}))\\) 是带有由超参数 \\(s\\) 控制尺度热核 (heat kernel) 的放缩矩阵 (scaling matrix)。  

图小波的逆 \\(\Psi_{s}^{-1}\\) 可以通过将 \\(\Psi_{s}(\lambda)\\) 中的 \\(g_{s}(\lambda)\\) 替换为 \\(g_{s}(-\lambda)\\) 得到。图小波中较小的索引对应低频成分，反之亦然。  

### 谱图小波基相比傅里叶基的三个好处
+ 对于现实场景中稀疏的网络，图小波基通常远比傅里叶基稀疏，悉数性使其在使用中计算效率更高。  
+ 在谱图小波中，由热核滤波器 (heat kernel filter) 得到的信号 \\(\Psi_{s}\\) 通常处于图上以及频域中的局部，可以通过调整 \\(s\\) 方便地控制局部邻域的范围，\\(s\\) 越小邻域越小。  
+ 由于在构建小波的过程中，特征值 \\(\lambda\\) 的信息会隐式包含在小波中，不会在再参数化过程中遇到信息损失问题。  

因而，使用图小波基 \\(\Psi_{s}\\) 的 SpGAT 层架构可以写作：  

$$X = H\Theta^T,$$
$$H' = \sigma(AGG(\Psi_{sL}\alpha_{L}\Psi_{sL}^{-1}X, \Psi_{sL}\alpha_{L}\Psi_{sL}^{-1}X)).$$  

在上式中为了进一步降低参数复杂度，特征变换阶段的参数进行了共享，即令 \\(\Theta_{L} = \Theta_{H} = \Theta\\)。  

通过显式地结合高低频特征，SpGAT 可以更好地处理全局信息。  

## 利用切比雪夫多项式对谱小波快速逼近

上一节中图谱小波变换的计算复杂度较高，因而采用切比雪夫多项式在不做特征分解的情况下快速逼近谱图小波。  

### 一条引理

\\(s\\) 为热核滤波器 \\(g_{s}(\lambda) = e^{-\lambda s}\\) 固定的放缩参数，\\(M\\) 是对放缩的小波所做切比雪夫多项式逼近的度 (\\(M\\) 越大逼近精度越高，但同时计算复杂度也越高)，图小波如下得到：  

$$\Psi_{s}(\lambda) = \frac{1}{2}c_{0,s} + \sum_{i=1}^M c_{i,s}T_{i}(\widetilde{L}),$$
$$c_{i,s} = 2e^{-s}J_{i}(-s),$$  

其中 \\(\widetilde{L} = \frac{2}{\lambda_{max}}L - I_{n}\\)，\\(T_{i}(\widetilde{L})\\) 是第 \\(i\\) 阶切比雪夫多项式逼近，\\(J_{i}(-s)\\) 是第一类 Bessel 函数。  

为了进一步加速计算，为 Bessel 函数 \\(J_{i}(s)\\) 构建一张查询表。将使用到切比雪夫多项式逼近的 SpGAT 称为 SpGAT-Cheby。  

![Fig 3](/images/2021/PRN25/3.png)

## 高低频边界 \\(d\\) 的选取

由图 3 可以看出在几个数据集上做好的表现都在 \\(d\\) 较小时得到，换句话说 SpGAT 中只有一小部分频率成分需要为处理为低频成分。  

## 高低频成分的影响

文中通过在测试阶段人为将 \\(\alpha_{L}\\) 或 \\(\alpha_{H}\\) 设为 0 观察图中高低频成分对分类精度的影响。  

![Tab 3](/images/2021/PRN25/T3.png)

## 特征可视化

![Fig 4](/images/2021/PRN25/4.png)
