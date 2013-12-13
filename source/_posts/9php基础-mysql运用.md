title: 9PHP基础-MYSQL运用
date: 2013-12-12 12:49:14
categories: PHP5
tags: [PHP5,MYSQL]
---
php访问mysql数据库服务器是通过安装相应的扩展模块完成的。

php的linux版本必须在编译时加上一个`--with-mysql`选项。php的windows版本则通过一个dll文件提供相应的扩展。

无论什么系统，都必须爱php.ini文件里面启用这个扩展确保php能够找到必要的DLL。

<!--more-->
##**连接mysql**

通过PHP脚本程序去管理mysql服务器中的数据，也必须先建立连接，然后才能通过PHP中的函数向服务器中发生sql查询语句。

1.php可以通过mysq的功能模块去连接mysql服务器，即调用`mysql_connect()`函数。通过mysql_close()函数断开与mysql服务器的连接。

{%codeblock lang:php%}
<?php
	$link = mysql_connect("localhost","root","123456");
	if (!$link) {
		die("连接失败：".mysql_error());
	}else{
		echo "服务器连接成功。";
	}

    echo mysql_get_client_info()."<br>";
    echo mysql_get_host_info()."<br>";
    echo mysql_get_proto_info()."<br>";
    echo mysql_get_server_info()."<br>";
    echo mysql_client_encoding()."<br>";
    echo mysql_stat();

    mysql_select_db("bookstore",$link) or die("不能选定bookstore：".mysql_error());

    mysql_close($link);
?>
{%endcodeblock%}

2.选定数据库

为了避免每次调用PHP的mysql扩展函数都指定目标数据库，事先使用`mysql_select_db()`函数为后续操作选择一个默认数据库。

	mysql_select_db("bookstore",$link) or die("不能选定bookstore：".mysql_error());

3.执行SQL

在PHP脚本中，只有把SQL命令作为一个字符串传递给`mysql_query()`函数，就会将其发送到mysql服务器并执行。

`mysql_query()`函数可用来执行DDL\DML\DQL\DCL等任何一种SQL命令。SQL执行成功，函数会返回一个非0值。失败返回false，并生产出错信息，可用使用mysql_errno和mysql_error 函数确定。



