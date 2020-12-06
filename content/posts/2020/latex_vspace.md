---
title: "压缩图表空间以调整 Latex 版面"
date: 2020-12-06T23:51:53+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "latex", "paper", "notes"]
draft: false
---

很多会议论文都有特定的模板格式，并且在篇幅上有所限制，为了尽可能地多写一点内容，可以考虑在图像、表格、公式中利用 `\vspace{}` 来压缩垂直距离。

例如：
    
    \begin{figure*}[t]
        \vspace{-1.0cm}
        \centering
        \includegraphics[width=2.0\columnwidth]{model_figure}
        \vspace{-0.5cm}
        \caption{...}
        \label{fig:model}
        \vspace{-0.5cm}
    \end{figure*}
