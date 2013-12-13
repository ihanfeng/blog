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


4.构造方法和析构方法

构造方法是对象创建完成后第一个被对象自动调用的方法，用来完成对象初始化工作。
析构方法是对象销毁前最后一个呗对象自动调用的方法，完成对象销毁前的清理工作。

构造方法格式：
{%codeblock lang:php%}
function __construct([参数列表]){
	//方法体，通常用来对成员属性进行初始化赋值
}
{%endcodeblock%}

在PHP中，同一个类中只能声明一个构造方法。没有构造方法重载。

析构方法格式：
{%codeblock lang:php%}
function __destruct(){
	//方法体，通常用来完成对象销毁前的清理任务
}
{%endcodeblock%}

PHP在对象被销毁前自动调用这个方法。

当堆内存段中的对象失去访问它的引用时，就不能被访问，称为垃圾对象。PHP有垃圾回收机制，当对象不能被访问时就会自动启动垃圾回收程序，收回对象在堆中占用的内存空间。析构方法在垃圾回收程序回收对象之前调用。

##**封装性**

1.设置私有成员

在声明成员属性或成员方法时，使用private关键字修饰就实现了对成员的封装。封装后的成员在对象外部不能被访问，但在对象内部的成员方法中可以访问到自己对象内部封装的成员属性和方法。
{%codeblock lang:php%}
<?php
class Person{
	private $name;
	private $age;

	function say(){
	//...
	}

	private function run(){
	//...
	}
}

?>
{%endcodeblock%}

2.私有成员的访问

对象中的成员属性被private关键字封装成私有后，就只能在对象内部的成员方法中使用，不能被对象外部直接赋值，也不能在对象外部直接获取私有属性的值。

那么该如何访问私有成员呢？我们可以在对象中声明一个访问私有属性的方法，再把这个方法通过public关键字设置为公有的访问权限。这样，在对象外部就可以将公有的方法作为访问接口，间接地访问内部私有成员。


3.__set()、__get()、__isset()、__unset()方法

PHP系统提供很多预定义方法，这些方法都需要在类中声明，只有在需要时才添加到类中。其作用、方法名称、使用参数列表、返回值都是PHP规定好的，并且都是以2个下划线开始的方法名称。

一般来说，把类中的成员属性都定义为private，但是对成员属性的读取和赋值操作很频繁，如果在类中为每个私有的属性都定义可以在对象的外部获取和赋值的公有方法是非常繁琐的。在PHP5.1后，预定义`__get()`,`__set()`两个方法，用来完成对所用私有属性都能获取和赋值的操作，以及检查私有属性是否存在的方法`__isset`和用来删除对象中私有属性的方法`__unset()`。

3.1 __set()的应用
{%codeblock lang:php%}
<?php
	class Person{
		private $name;
		private $age;
		private $sex;

		function __construct($name="",$sex="男",$age=20){
			$this->$name = $name;
			$this->$sex = $sex;
			$this->$age = $age;
		}

		public function __set($propertyName,$propertyValue){
			if($propertyName == "sex"){
				if(!($propertyValue == "男" || $propertyValue == "女")){
					return;
				}
			}

			if($propertyName == "age"){
				if ($propertyValue >150 || $propertyValue<0) {
					return;
				}
			}

			//根据参数决定为哪个属性被赋值，传入不同的成员属性名，赋上传入相应的值
			$this->$propertyName = $propertyValue;
		}

		public function say(){
			echo "我的名字是：".$this->name."，性别:".$this->sex."，年龄：".$this->age."<br>";
		}
	}

	$p1 = new Person("张三","男",25);

	$p1->name = "李四";
	$p1->sex = "女";
	$p1->age = 30;

	$p1->say();
?>
{%endcodeblock%}


3.2 __get()应用
{%codeblock lang:php%}
<?php
	class PersonTwo{
		private $name;
		private $age;
		private $sex;

		function __construct($name="",$sex="男",$age=20){
			$this->$name = $name;
			$this->$sex = $sex;
			$this->$age = $age;
		}

		public function __set($propertyName,$propertyValue){
			if($propertyName == "sex"){
				if(!($propertyValue == "男" || $propertyValue == "女")){
					return;
				}
			}

			if($propertyName == "age"){
				if ($propertyValue >150 || $propertyValue<0) {
					return;
				}
			}

			//根据参数决定为哪个属性被赋值，传入不同的成员属性名，赋上传入相应的值
			$this->$propertyName = $propertyValue;
		}

		public function __get($propertyName){
			if ($propertyName == "sex") {
				return "保密";
			}else if($propertyName == "age"){
				if ($this->age >30) {
					return $this->age-10;
				}else{
					return $this->$propertyName;
				}
			}else{
				return $this->$propertyName;
			}
		}

		public function say(){
			echo "我的名字是：".$this->name."，性别:".$this->sex."，年龄：".$this->age."<br>";
		}
	}

	$p1 = new PersonTwo("张三","男",25);

	$p1->name = "李四";
	$p1->sex = "女";
	$p1->age = 30;

	// $p1->say();

	echo "姓名：".$p1->name."<br>";
	echo "年龄：".$p1->age."<br>";
	echo "性别：".$p1->sex."<br>";
?>
{%endcodeblock%}

##**继承性**

感觉和JAVA思想都差不多的。

PHP中不能定义重名的函数，也包括不能再同一个类中定义重名的方法，所以也就没有方法重载，在子类中可以定义和父类同名的方法，因为父类的方法已经在子类中存在，所以子类中就可以把从父类中继承过来的方法重写。


##**常见关键字和魔术方法**

1.final

可以在类或类中方法前面，但不能使用final标识成员属性。

作用：

- 使用final标识的类，不能被继承。
- 在类中使用final标识的成员方法，在子类中不能被覆盖。

2.static

使用static关键字可以将类中的成员标识为静态的，既可以用来标识成员属性，也可以用来标识成员方法。

类中的静态成员是不需要对象而是直接用类名来访问。

	类名::静态成员属性名;
	类名::静态成员方法名();

或则可以是使用`self`关键字来访问其他静态成员。

	self::静态成员属性名;
	self::静态成员方法名();

3.单例设计模式

保证在面向对象编程中，一个类只能有一个实例对象存在。

4.const关键字

要将类中的成员属性定义为常量，则只能使用const关键字。

5.instanceof关键字

该关键字可以确定一个对象是类的实例、类的子类，还是实现了某个特定接口，并进行操作。

6.colone对象

7.自动加载类：__autoload()

格式：
{%codeblock lang:php%}
<?php
	//声明一个自动加载类的魔术方法__autoload()
	function __autoload($className){
		//在方法中使用include包含类所在的文件
		include(strtolower($className).".class.php");
	}

	$obj = new User(); //User类不存在则自动调用__autoload()函数，将类名'User'作为参数传入
	$obj2 = new Shop();
?>
{%endcodeblock%}

8.对象串行化

对象时在内存中存储的数据类型，生命周期通常伴随着对象的程序的终止而终止。有时候，可能需要将对象的状态保存下来，需要是再将对象恢复。对象通过写出描述自己状态的数值来记录自己，这个过程称对象的串行化（Serialization）。串行化就是把整个对象转换为二进制字符串。

必须串行化：
- 对象需要在网络中传输时，将对象串行化成二进制串后在网络中传输
- 对象需要持久保存时，将对象串行化后写入文件或是数据库中

串行化：serialize()

反串行化：unserialize()


##**抽象类和接口**

抽象类是一个规范，子类要求遵守父类的规范。当子类继承抽象类后，就必须把抽象类中的抽象方法安装子类自己的需要去实现。子类必须实现父类中的所有抽象方法。

PHP支持单继承。抽象类中的所有方法都是抽象方法，那么接口声明的方法必须都是抽象方法，接口中不能声明变量，只能使用const关键字声明为常量的成员属性，且接口中所有成员都必须有public访问权限。


##**多态性**


