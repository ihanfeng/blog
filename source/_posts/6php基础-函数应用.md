title: 6.PHP基础-函数应用
date: 2013-12-11 15:44:13
categories: PHP5
tags: [PHP5,函数应用]
---
##**自定义函数**

1.函数声明

格式：
{%codeblock lang:php%}
function 函数名([参数1,参数2,...参数n]){
	函数体;
	return 返回值;
}
{%endcodeblock%}
<!--more-->
2.函数调用

不管是自定义函数还是系统函数，如果函数不被调用，就不会执行。只要在需要使用函数的位置，使用函数名称和参数列表进行调用。函数被调用后开始执行函数体中的代码，执行完毕返回到调用的位置继续向下执行。

格式：
{%codeblock lang:php%}
<?php
	table();

	function table(){

	}

	table();
?>
{%endcodeblock%}

3.函数参数

参数列表由0个、1个或多个参数组成。每个参数是一个表达式，用逗号分隔。

4.函数返回值

函数执行后的结果返回给调用者。

##**PHP变量的范围**

大部分PHP变量只有一个单独的使用范围，也包含include和require引入的文件。

1.局部变量

内部变量，是在函数内部声明的变量，其作用域仅限于函数内部，离开该函数后使用这个变量就是非法。

2.全局变量

外部变量，是在函数外部定义的，其作用域从变量定义开始到本程序文件的末尾。全局变量不会自动设置为可用。在php中，由于函数可用视为单独的程序片段，所以局部变量会覆盖全局变量的能见度，因此在函数中并无法直接调用全局变量。

若要使用全局变量，必须利用global关键字定义目标变量，以告诉函数主体此变量为全局变量。

{%codeblock lang:php%}
<?php
	$one = 200;
	$two = 100;

	function demo(){
		global $one,$two;

		echo "....";
	}
	
?>
{%endcodeblock%}

还可以使用特殊的PHP自定义$GLOBALS数组。
{%codeblock lang:php%}
<?php
	$one = 200;
	$two = 100;

	function demo(){
		$GLOBALS['two'] = $GLOBALS['two'] + $GLOBALS['one'];

		echo "....";
	}
	
?>
{%endcodeblock%}


3.静态变量

使用关键字`static`。


##**声明及应用各种形式的PHP函数**

1.常规参数的函数，即实参和形参的个数相等。

2.伪类型参数函数

3.引用参数函数

	void funName(array &arg)

4.默认参数的函数

定义函数时声明了参数，调用时函数没有指定参数或少指定参数，就会使用参数的默认值，默认值必须是常量表达式。

5.可变个数参数的函数

6.回调函数

7.递归函数


