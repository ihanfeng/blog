title: 7PHP基础-数组与数据结构
date: 2013-12-11 16:18:46
categories: PHP5
tags: [PHP5,数组,数据结构]
---
##**数组定义**

两种方法：

- 直接为数组元素赋值即可声明数组

{%codeblock lang:php%}
<?php
	$info[0] = 1;
	$info[1] = "zhangsan";
	$info[2] = "fuzhou";
	$info[3] = "13599552233";
	$info[4] = "123@qq.com";
?>
{%endcodeblock%}

- 使用array()函数声明数组

{%codeblock lang:php%}
<?php
	$arrayName = array(1,"zhangsan","fuzhou","456465465","465@qq.com");
?>
{%endcodeblock%}
<!--more -->

##**数组遍历**

1.使用for循环

2.使用foreach

##**预定义数组**

1.服务器遍历：$_SERVER
2.环境变量：$_ENV
3.URLGET变量：$_GET
4.HTTPPOST变量：$_POST
5.request变量：$_REQUEST
6.HTTP文件上传变量：$_FILE
7.HTTPCOOKIES:$_COOKIE
8.SESSION：$_SESSION
9.Global：$GLOBALS

##**数组相关处理函数**



