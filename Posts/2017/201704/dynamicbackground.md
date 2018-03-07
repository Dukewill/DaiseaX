---
title: 给 Hexo 主题添加动态背景
date: 2017-04-20 21:00:07
tags: [教程, 主题, Hexo, themes]
categories: [教程]
banner: https://flk1iw.dm2301.livefilestore.com/y4m-NKuANMcGvAvJMCADj6YurfR2cH1t_NBUNiXaHaFC-N6bWhEqynB6gKrJn9HHqhjVCRUYFe9ekJF5e8j9gtnoCUUXnKY6I08BTSzoKvpBgg8P4EQEUcwDZ19XMg1HSxM8_y92ETzi1o6aBILGntcYNxBnd0Td8FDxOFLJN9Urc0rg9dtqtgyvm2BE9YdAR7RYVJp5tCPCs62TOwSsDGqmA?width=1600&height=727&cropmode=none
thumbnail:
---
一步一步来。
#### 修改 _layout.ejs
打开`themes/主题名称/layout/_layout.ejs`
在 < /body>之前添加代码（不要放在< /head>后）。
```
<% if (theme.canvas_nest) { %>
<script type="text/javascript"
color="0,0,0" opacity='0.4' zIndex="-2" count="66" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
<% } %>
```
以上代码格式并不绝对，可以参照自己所用主题的 _layout 中有 if 的那部分代码。多尝试，并不难。
<!--more-->
###### 配置项说明
```
color：线条颜色, 默认：'0,0,0'；三个数字分别为（R,G,B）
opacity：线条透明度（0~1）, 默认：0.5
count：线条的总数量，默认：150
zIndex：背景的z-index属性，css属性用于控制所在层的位置，默认：-1
```
#### 修改配置文件
打开`themes/主题名称/_config.yml`，随意找一行，添加代码：
```
# --------------------------------------------------------------
# background settings
# --------------------------------------------------------------
# add canvas-nest effect
# see detail from https://github.com/hustcc/canvas-nest.js
canvas_nest: true
```
然后运行`hexo s`，在浏览器的地址栏输入`localhost:4000`就能看到效果了。

这里的代码主要参照：**[距离](https://shenzekun.github.io/hexo%E5%A6%82%E4%BD%95%E6%B7%BB%E5%8A%A0%E5%8A%A8%E6%80%81%E8%83%8C%E6%99%AF.html)，特此感谢！**
