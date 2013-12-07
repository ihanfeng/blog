title: Markdown学习指南
date: 2013-12-05 09:32:24
categories: markdown
tags: [markdown指南]
---
##**1、Markdown介绍**

Markdown是一种轻量级标记语言，允许人们使用易读易写的纯文本格式编写文档，然后转换成有效的XTHML文档。

**优点：**

- 纯文本，所以兼容性极强，可以用所有文本编辑器打开。
- 让你专注于文字而不是排版。
- 格式转换方便，Markdown 的文本你可以轻松转换为 html、电子书等。
- Markdown 的标记语法有极好的可读性。

<!-- more -->

##**2、Markdown简明语法**

###**基本符号**
- `*`,`-`,`+` 3个符号效果都一样，这3个符号被称为 Markdown符号
- 空白行表示另起一个段落
- ` ` ` 是表示inline代码，`tab`是用来标记 代码段，分别对应html的`code`，`pre`标签

###**换行**
- 单一段落(`<p>`)用一个空白行
- 连续两个空格 会变成一个 <br>
- 连续3个符号，然后是空行，表示 hr横线

###**标题**
在Markdown中，只需要在文本前面加上 `# `即可。同理、你还可以增加二级标题、三级标题、四级标题、五级标题和六级标题，总共六级，只需要增加  `#`即可，标题字号相应降低。

	# 这是一级标题
	## 这是二级标题


注：# 和「一级标题」之间建议保留一个字符的空格，这是最标准的 Markdown 写法。
<img src="/img/2013/12/markdown-biaoti.jpg"/>

###**引用**

在我们写作的时候经常需要引用他人的文字，这个时候引用这个格式就很有必要了，在Markdown中，你只需要在你希望引用的文字前面加上 > 就好了，例如：

> 大风起兮云飞扬，壮士去兮不复返！

注：> 和文本之间要保留一个字符的空格。

![](/img/2013/12/markdown-yinyong.jpg)

###**列表**

列表格式也很常用，在Markdown中，你只需要在文字前面加上 `-`即可。


	- 文本1
	- 文本2
	- 文本3


如果你希望有序列表，也可以在文字前面加上 `1. 2. 3.`就可以了，例如：

	1. 文本1
	2. 文本2
	3. 文本3


注：`-`、`1.`和文本之间要保留一个字符的空格。

<img src="/img/2013/12/markdown-liebiao.jpg"/>


###**链接**

在 Markdown 中，插入链接不需要其他按钮，你只需要使用 `[显示文本](链接地址)` 这样的语法即可，例如：

[Hello World](http://zhdevelop.github.io)

###**插入图片**

和插入链接类似，你只需要使用 `![](图片链接地址)` 这样的语法即可，例如：

	![](/img/markdown-liebiao.jpg) (推荐)

或者

	<img src="/img/markdown-liebiao.jpg"/>

###**粗体和斜体**

Markdown 的粗体和斜体也非常简单，用两个 `*` 包含一段文本就是粗体的语法，用一个 `*` 包含一段文本就是斜体的语法。

	*大风起兮云飞扬，**壮士**去兮不复返！*

*大风起兮云飞扬，**壮士**去兮不复返！*

###**特殊符号**
- 用` \ `来转义，表示文本中的markdown符号
- 可以在文本种直接使用html标签，但是要注意在使用的时候，前后加上空行
- 文本前后各加一个符号，表示斜体


###**表格**
可以使用html格式进行编写。
在线处理：[http://pressbin.com/tools/excel_to_html_table/index.html](http://pressbin.com/tools/excel_to_html_table/index.html)

参考链接：[http://www.ituring.com.cn/article/3452](http://www.ituring.com.cn/article/3452)

##**参考文献：**
1.[鲁塔弗：markdown 简明语法](http://lutaf.com/markdown-simple-usage.htm)
2.[图灵社区：怎样使用Markdown](http://www.ituring.com.cn/article/23)
3.[简书：献给写作者的 Markdown 新手指南](http://jianshu.io/p/q81RER)
4.[官方文档(中文版)：Markdown 语法说明](http://wowubuntu.com/markdown/#p)

##**编辑器推荐:**
- [简书](http://jianshu.io/)
- []MaDe (Chrome插件)](https://chrome.google.com/webstore/detail/made/oknndfeeopgpibecfjljjfanledpbkog)


