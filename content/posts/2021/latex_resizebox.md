---
title: "Latex 调整表格宽度与高度"
date: 2021-12-13T12:01:57+08:00
categories: ["Things About Latex & MD"]
tags: ["cv", "notes", "deep learning", "latex", "paperwriting"]
draft: false
---

有时会遇到表格超出页面正常宽度的情况，可使用 `\resizebox` 命令进行调整。  

```
\begin{table}
\centering
\resizebox{\columnwidth}{20mm}{
\begin{tabular}{l|ccc}
\toprule
Method & Top-1 & Top-5 & Top-10 \\
\midrule
DSSL \small{$_{MM21}$} \tiny{\cite{dssl}} & 59.98 & 80.41 & \textbf{87.56} \\
\bottomrule
\end{tabular}}
\caption{xxxxxxxx.}
\label{tab:example}
\end{table}
```

注：命令加在 tabular 外一层，高度需要自己调整到合适的值，目前没有找到可以自适应于宽度的方法，如果后面发现了再更新。  
