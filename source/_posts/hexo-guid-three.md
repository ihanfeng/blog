title: Hexo教程三：Hexo配置
date: 2013-12-05 12:31:03
categories: hexo
tags: [hexo,hexo配置]
---
前面的章节，我们学会了搭建hexo博客站点。现在我们来进入配置环节。

hexo的配置文件有2个。

- `_config.yml` 整站配置
- `themes\light_config.yml` 主题配置

<!-- more -->

###**1、整站配置文件 _config.yml**

{%codeblock lang:yaml %}
# Hexo Configuration
## Docs: http://zespia.tw/hexo/docs/configure.html
## Source: https://github.com/tommy351/hexo/

# Site
#站点名字
title: Hello World
#站点副标题
subtitle: Code如痴如醉，关注移动互联网，关注电子商务
#站点描述
description: JAVA码农、工作学习笔记、各种IT狂
#作者
author: hanfeng
#邮箱
email: zhdevelop@gmail.com
#语言
language: zh-CN

# URL 绑定域名配置
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://zhdevelop.github.io/
root: /
permalink: :year/:month/:day/:title/
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code

# Writing 文章布局、写作格式定义，默认即可
new_post_name: :title.md # File name of new posts
default_layout: post
auto_spacing: false # Add spaces between asian characters and western characters
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
max_open_file: 100
multi_thread: true
filename_case: 0
render_drafts: false
highlight:
  enable: true
  line_number: true
  tab_replace:

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Archives
## 2: Enable pagination
## 1: Disable pagination
## 0: Fully Disable
archive: 2
category: 2
tag: 2

# Server
## Hexo uses Connect as a server
## You can customize the logger format as defined in
## http://www.senchalabs.org/connect/logger.html
port: 4000
logger: false
logger_format:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: MMM D YYYY
time_format: H:mm:ss

# Pagination 每页显示文章数，可以自定义，我将10改成了5
## Set per_page to 0 to disable pagination
per_page: 5
pagination_dir: page

# Disqus Disqus插件，我们会替换成“多说”，不修改
disqus_shortname:

# Extensions 配置站点所用主题和插件
## Plugins: https://github.com/tommy351/hexo/wiki/Plugins
## Themes: https://github.com/tommy351/hexo/wiki/Themes
theme: light
exclude_generator:

# Deployment 站点部署到github要配置
## Docs: http://zespia.tw/hexo/docs/deploy.html
deploy:
  type: github
  repository: https://github.com/zhdevelop/zhdevelop.github.io.git
  branch: master

#RSS插件 添加sitemap
  plugins:
	- hexo-generator-feed
	- hexo-generator-sitemap

{%endcodeblock%}

###**2、主题配置文件 light_config.yml**

{%codeblock lang:yaml %}
#站点右上角导航栏
menu:
  Home: /
  Archives: /archives
  About: /about

#站点右边栏
widgets:
- search
- category
- tagcloud
- weibo
- blogroll


excerpt_link: 阅读全文

#右边栏要显示twitter展示的话，需要在此设置
twitter:
  username: zhdevelop
  show_replies: false
  tweet_count: 5

#SNS分享，身在天朝，当然用“百度分享”
addthis:
  enable: true
  pubid:
  facebook: true
  twitter: true
  google: true
  pinterest: false

#图片效果，默认
fancybox: true

#要使用google_analytics进行统计的话，这里需要配置ID
google_analytics: UA-41331600-2
#生成RSS，需要配置路径
rss: /atom.xml

comment_provider: facebook
# Facebook comment
facebook:
  appid: 123456789012345
  comment_count: 5
  comment_width: 840
  comment_colorscheme: light
  {%endcodeblock%}

###**参考文献：**
[zipperary's Blog](http://zipperary.com/2013/05/29/hexo-guide-3/)