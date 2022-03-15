---
title: "Hugo 升级后编译时 can't evaluate field RSSLink in type *hugolib.pageState 问题解决方案"
date: 2022-03-16T01:08:48+08:00
categories: ["Toss About Blog"]
tags: ["blog"]
draft: false
---

## 问题描述

升级了 Hugo 框架之后，编译博客时遇到如下报错：
```
Error: Error building site: failed to render pages: render of "page" failed: execute of template failed: template: page/single.html:3:5: executing "page/single.html" at <partial "head.html" .>: error calling partial: "/home/xxxx/themes/LeaveIt/layouts/partials/head.html:42:8": execute of template failed: template: partials/head.html:42:8: executing "partials/head.html" at <.RSSLink>: can't evaluate field RSSLink in type *hugolib.pageState
```

## 原因分析

其实翻看更新前的 log 可以发现存在这样一个 warning：  
```
WARN 2021/09/28 11:38:44 Page.RSSLink is deprecated and will be removed in a future release. Use the Output Format's link, e.g. something like:
    {{ with .OutputFormats.Get "RSS" }}{{ .RelPermalink }}{{ end }}
```
因此，主要原因在于 `Page.RSSLink` 在新版本中被弃用，需要进行替换。  

## 解决办法

首先找到报错中所指的 `head.html` 文件，找到如下这几行：  

```
{{ if .RSSLink }}
  <link href="{{ .RSSLink }}" rel="alternate" type="application/rss+xml" title="{{ .Site.Title }}" />
  <link href="{{ .RSSLink }}" rel="feed" type="application/rss+xml" title="{{ .Site.Title }}" />
{{ end }}
```

需要将其替换为如下几行代码：  
```
{{ $foo := .Site.Title }}
{{ with .OutputFormats.Get "RSS" }}
  <link href="{{ .RelPermalink }}" rel="alternate" type="application/rss+xml" title="{{ $foo }}" />
  <link href="{{ .RelPermalink }}" rel="feed" type="application/rss+xml" title="{{ $foo }}" />
{{ end }}
```
这里为 `.Site.Title` 声明一个变量是必要的，否则编译时会报错。  

编译后发现又出现了如下错误：  
```
Error: Error building site: failed to render pages: render of "taxonomy" failed: "/home/xxxx/themes/LeaveIt/layouts/_default/terms.html:19:25": execute of template failed: template: _default/terms.html:19:25: executing "content" at <.URL>: can't evaluate field URL in type page.Page

```
根据报错信息可以定位到出错文件 `term.html`，同样查看更新前 log 可以发现如下 warning：  
```
WARN 2021/09/28 11:38:44 Page.URL is deprecated and will be removed in a future release. Use .Permalink or .RelPermalink. If what you want is the front matter URL value, use .Params.url
```

由此可知也是版本更新后的弃用问题，只需要将文件中所有的 `.URL` 替换为 `.RelPermalink` 即可。  

再次编译，发现出现了新的报错如下：  
```
Error: Error building site: failed to render pages: render of "home" failed: "/home/xxxx/themes/LeaveIt/layouts/rss.xml:12:23": execute of template failed: template: rss.xml:12:23: executing "rss.xml" at <.URL>: can't evaluate field URL in type *hugolib.pageState
```

可以看出是由于修改了 `.URL` 后没有在 `rss.xml` 中对应修改导致的，找到该文件，修改对应内容后再次编译：  
```
Start building sites …
hugo v0.93.2+extended linux/amd64 BuildDate=unknown

|  EN
-------------------+-------
Pages            |  744
Paginator pages  |  152
Non-page files   |    0
Static files     | 1893
Processed images |    0
Aliases          |  158
Sitemaps         |    1
Cleaned          |    0

Total in 5539 ms
```

编译成功通过，问题解决了。  
