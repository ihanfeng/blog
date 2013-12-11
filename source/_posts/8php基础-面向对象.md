title: 8PHP基础-面向对象
date: 2013-12-11 16:43:28
categories: PHP5
tags: [php5,面向对象]
---
##**抽象一个类**
1.类的声明

格式：

	[修饰关键字] class 类名{
		类中成员;
	}

例子：

	class Person{
		成员属性：
			姓名、性别、年龄、身高、体重、电话、住址等
		成员方法：
			说话、学习、吃饭、开车等
	}

<!-- more -->
2.成员属性

在类中直接声明变量就称为成员变量，可以声明多个变量，每个变量存储不同的属性信息。
{%codeblock lang:php%}
class Person{
		var $name;
		var $age;
		var $sex;
	}
{%endcodeblock%}

3.成员方法

在对象中需要声明一些可以操作本对象成员属性的一些方法，来完成对象的一些行为。在类中直接声明的函数称为成员方法，可以在类中声明多个函数。
{%codeblock lang:php%}
class Person{
	function say(){
	//...
	}

	private function run(){
	//...
	}
}
{%endcodeblock%}


##**通过类实例化对象**

面向对象程序的单位就是对象，对象时通过类的实例化出来的。

1.实例化对象

使用new关键字并在后面加上一个和类名同名的方法。

{%codeblock lang:php%}
<?php
class Person{
	var $name;
	var $age;

	function say(){
	//...
	}

	private function run(){
	//...
	}
}

	//实例化对象
	$p1 = new Person();
	$p2 = new Person();

?>
{%endcodeblock%}

2.对象类型在内存中的分配

3.对象成员的访问

{%codeblock lang:php%}
<?php

	//初始化赋值
	$p1->name = "zhangsan";

	//访问
	echo "$p1->name";
	$p1->say();
?>
{%endcodeblock%}

