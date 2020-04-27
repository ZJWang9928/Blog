---
title: "Hugo实现LaTex渲染"
date: 2020-04-15T20:56:12+08:00
categories: ["Toss About Blog"]
tags: ["blog"]
draft: false
---

Hugo框架本身并不支持LaTex，但是可以通过javascript进行渲染。Hugo官网上提供了多种方法，权衡之后我选择了较为简单的KaTex，而不是MathJax。   

# 安装
从[这里](https://github.com/KaTeX/KaTeX/releases)下载katex的包，解压后放在`static/js`文件夹中，并在`head.html`中加入以下代码    

    <!-- KaTeX -->
    <link rel="stylesheet" href="/js/katex/katex.min.css" >
    <script src="/js/katex/katex.min.js" > </script>
    <script src="/js/katex/contrib/auto-render.min.js" ></script>

    <script>
        document.addEventListener("DOMContentLoaded", function() {
          renderMathInElement(document.body);
        });
    </script>

# 使用
尽管[KaTeX的github](https://github.com/KaTeX/KaTeX/blob/master/contrib/auto-render/README.md)上是如下给出auto-render的默认值的：    

    [
      {left: "$$", right: "$$", display: true},
      {left: "\\[", right: "\\]", display: true},
      {left: "\\(", right: "\\)", display: false}
    ]

但实际上，三种标识符都成立，其中前两种将数学公式以block形式展现，第三种以inline形式展现。使用中只需要将left和right放在数学公式两侧即可。    
