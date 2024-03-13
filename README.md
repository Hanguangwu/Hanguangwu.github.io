# README

模板

```txt
---
title: "古文"
date: 2024-05-02T18:34:25+08:00
lastmod: 2024-05-03T22:54:22+08:00
draft: false
math: true
categories:
- 古文
tags:
- 古文
# description->需要自己编写的文章描述，是搜索引擎呈现在搜索结果链接下方的网页简介，建议设置
description: ""
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
draft: false # 是否为草稿
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
---
```
```txt
---
title: "英剧"
date: 2024-05-03T22:54:22+08:00
lastmod: 2024-05-03T22:54:22+08:00
author: ["RM"]
categories:
- 分类1
- 分类2
tags:
- 标签1
- 标签2
# summary->在列表页展现的摘要内容，自动生成，内容默认前70个字符，可通过此参数自定义，一般无需专门设置
summary: ""
# description->需要自己编写的文章描述，是搜索引擎呈现在搜索结果链接下方的网页简介，建议设置
description: ""
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
draft: false # 是否为草稿
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示当前路径
cover:
    image: ""
    caption: ""
    alt: ""
    relative: false
---

```


### shortcode

数学公式

[参考资料](https://adityatelange.github.io/hugo-PaperMod/posts/math-typesetting/)

```md
Math Typesetting
A brief guide to setup KaTeX
March 8, 2019
 · 1 min · Hugo Authors | 
Suggest Changes
Table of Contents
Mathematical notation in a Hugo project can be enabled by using third party JavaScript libraries.

In this example we will be using KaTeX

Create a partial under /layouts/partials/math.html
Within this partial reference the Auto-render Extension or host these scripts locally.
Include the partial in your templates (extend_head.html) like so:
refer ISSUE #236
{{ if or .Params.math .Site.Params.math }}
{{ partial "math.html" . }}
{{ end }}
To enable KaTex globally set the parameter math to true in a project’s configuration
To enable KaTex on a per page basis include the parameter math: true in content files
Note: Use the online reference of Supported TeX Functions

```

PPT

```md
{{< ppt src="ppt网址" >}} 
```
Bilibili视频

```md
{{< bilibili BV1xW4y1a7NK >}}  
# BV1Ab4y117G2 指的是 bilibili 链接中的 bvid
# 如果有集数（默认第一集），例如要播放第5集，则这样使用：{{< bilibili BV1xW4y1a7NK 5 >}}
```
youtube视频

```md
{{< youtube WsptdUFthWI >}}  
```
内连接

```md
{{< innerlink src="posts/tech/go_slice_map_thread_safety.md" >}}  
# 注意：结尾要加md，开头不用加域名

# 卡片获取的文章长度默认是70，需要在config.yaml配置文件添加 summaryLength: 140，即设置为140
```
豆瓣

```md
{a{< douban src="直接放网址如：https://book.douban.com/subject/20394150/" >}}
```


### Front Matter

```md
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
lastmod: {{ .Date }}
author: ["作者"]
categories:
- 分类1
- 分类2
tags:
- 标签1
- 标签2
# summary->在列表页展现的摘要内容，自动生成，内容默认前70个字符，可通过此参数自定义，一般无需专门设置
summary: ""
# description->需要自己编写的文章描述，是搜索引擎呈现在搜索结果链接下方的网页简介，建议设置
description: ""
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
draft: false # 是否为草稿
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示当前路径
cover:
    image: ""
    caption: ""
    alt: ""
    relative: false
---

此处内容将会出现在摘要（summary）里

<!--\more--> # 此处的“\”用于转义，否则无法正常显示，实际使用须删去。

此处开始为正文


```




