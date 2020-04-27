---
title: "Hugo LeaveIt添加网站流量统计"
date: 2020-01-24T16:28:50+08:00
categories: ["Toss About Blog"]
tags: ["blog"]
draft: false
---
[不蒜子](http://busuanzi.ibruce.info/ "不蒜子")是一个通过仅仅两行代码实现的网页流量计数器  
    

1.在themes/layouts/partials/head.html文件中引入不蒜子js文件
    

    <!-- 不蒜子 -->
    <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>

    
2.在页面添加统计代码，在/themes/layouts/partials/footer.html中添加如下代码
    

    <span id="busuanzi_container_site_pv">
        本站访问量：<span id="busuanzi_value_site_pv"></span>次
    </span>
    &nbsp;
    <span id="busuanzi_container_site_uv">
        您是本站第 <span id="busuanzi_value_site_uv"></span> 位访问者
    </span>
    

3.在themes/layouts/\_default/single.html中添加以下代码
    

    <h5 id="wc" style="font-size: 1rem;text-align: center;">{{ .FuzzyWordCount }} Words|Read in about {{ .ReadingTime }} Min|本文总阅读量<span id="busuanzi_value_page_pv"></span>次</h5>
    

可根据个人喜好选择放在文章头部或尾部   
