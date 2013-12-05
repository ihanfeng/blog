title:  Hexo教程四：Hexo优化
date: 2013-12-05 12:59:37
categories: hexo
tags: [hexo,hexo优化]
---
##**添加“多说”评论**

hexo默认使用国外比较流行的disqus，不过，按照“因地制宜”的原则，我们修改为国内用的多又好用的“多说”评论系统。步骤非常简单:

在多说进行注册，获得通用代码。

将通用代码粘贴到`themes\light\layout\_partial\comment.ejs`里面，如下：

{%codeblock lang:yaml %}
<% if ( page.comments){ %>
<section id="comment">
通用代码
</section>
<% } %>
{%endcodeblock%}

<!-- more -->

##**添加“百度分享”**

到[百度分享](http://share.baidu.com/code)获得代码，在`themes/light/layout/_partial/article.ejs`中，将`<%-partial('post/share')%>`删掉，替换为百度分享的代码。

##**添加小图标**
在themes/light/layout/_partial/head.ejs里将`<link href="<%- config.root %>favicon.png" rel="icon">`替换为`<link href="<%- config.root %>favicon.ico" rel="icon" type="image/x-ico">`。将favicon.ico图标文件放在source目录下。制作图标的网站，[http://www.faviconer.com](http://www.faviconer.com)。

##**添加分类、标签云widget**
很简单，在`themes/light/_config.yml`中，添加如下：
{%codeblock lang:yaml %}
	widgets:
	- category
	- tagcloud
{%endcodeblock%}

##**添加友情链接widget**
在`themes/light/layout/_widge`t中新建名为`blogroll.ejs`的文件，编辑内容如下：
{%codeblock lang:html %}
<div class="widget tag">
<h3 class="title">友情链接</h3>
<ul class="entry">
<li><a href="http://zipperary.com/" title="Zippera's Blog">Zippera</a></li>
</ul>
</div>
{%endcodeblock%}
在`themes/light/_config.yml`中，添加如下：
{%codeblock lang:yaml %}
widgets:
- blogroll
{%endcodeblock%}

##**生成post时默认生成categories配置项**

在`scaffolds/post.md`中，添加一行`categories:`。同理可应用在`page.md`和`photo.md`。

##**添加新浪微博widget(微博秀)**

去新浪微博开放平台设置和生成微博秀代码。
在`themes/light/layout/_widget`中新建名为`weibo.ejs`的文件，将刚才的代码直接保存到这里。
在`themes/light/_config.yml`中，添加如下：
{%codeblock lang:yaml %}
widgets:
- weibo
{%endcodeblock%}

##**导航栏添加“关于”**
	hexo new page "about"
到`source/about/index.md`编辑内容。
在`themes/light/_config.yml`中，添加如下：
{%codeblock lang:yaml %}
menu:
  关于: /about
{%endcodeblock%}

##**主页文章显示摘要**
编辑md文件的时候，在要作为摘要的文字后面添加`<!--more-->`即可。