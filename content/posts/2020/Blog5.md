---
title: "Hugo LeaveIt主题修改代码块底色"
date: 2020-04-15T21:47:18+08:00
categories: ["Toss About Blog"]
tags: ["blog"]
draft: false
---

![before](/images/2020/04/blog5/1.jpg)
如上图，LeaveIt主题在浅色模式下默认的代码块背景色是纯白色，和文字撞色导致观感较差。
    
可对文件`themes/LeaveIt/assets/css/_common/_page/post.scss`从第86行起的内容做如下修改：     
    
        code,
        pre {
            padding: 7px;
            font-size: 13px;
            font-family: Consolas, Monaco, Menlo, Consolas, monospace;
            word-break: break-all;
            word-wrap: break-word;
        }

        code:not([class]) {
            padding: 1px 6px;
            margin: 0 2px;
            color: #ffffff;     // 避免改后inline代码块颜色冲突
            // background: $light-code-notclass-background-color;    // 原本内容
            background: #322931;    // 改成与底色相同
            border-radius: 5px;

            .dark-theme &:not([class]) {
                background: $dark-code-notclass-background-color;
            }
        }

改完后看起来顺眼多了。  
![after](/images/2020/04/blog5/2.jpg)
