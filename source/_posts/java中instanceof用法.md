title: Java中instanceof用法
date: 2013-12-06 14:57:20
categories: JAVA
tags: [java,instanceof]
---
Java 中的`instanceof` 运算符是用来在运行时指出对象是否是特定类的一个实例。instanceof通过返回一个布尔值来指出，这个对象是否是这个特定类或者是它的子类的一个实例。
<!-- more -->
**用法：**

	result = object instanceof class

**参数：**

- Result：布尔类型。
- Object：必选项。任意对象表达式。
- Class：必选项。任意已定义的对象类。

**说明：**
如果 object 是 class 的一个实例，则 instanceof 运算符返回 true。如果 object 不是指定类的一个实例，或者 object 是 null，则返回 false。

实例：
{%codeblock lang:java%}
public interface IObject { 
} 

public class Foo implements IObject{ 
} 

public class Test extends Foo{ 
} 

public class MultiStateTest { 
        public static void main(String args[]){ 
                test(); 
        } 

        public static void test(){ 
                IObject f=new Test(); 
                if(f instanceof java.lang.Object)System.out.println("true"); 
                if(f instanceof Foo)System.out.println("true"); 
                if(f instanceof Test)System.out.println("true"); 
                if(f instanceof IObject)System.out.println("true"); 
        } 
}
{%endcodeblock%}

结果：

	true 
	true 
	true 
	true