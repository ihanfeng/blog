title: 重温spring3-DI配置
date: 2013-12-09 13:14:06
categories: Spring3
tags: [Spring3,DI配置]
---
##**依赖和依赖注入**

传统应用程序设计中所说的依赖一般指“类之间的关系”，那先让我们复习一下类之间的关系：

- **泛化**：表示类与类之间的继承关系、接口与接口之间的继承关系；
- **实现**：表示类对接口的实现；
- **依赖**：当类与类之间有使用关系时就属于依赖关系，不同于关联关系，依赖不具有“拥有关系”，而是一种“相识关系”，只在某个特定地方（比如某个方法体内）才有关系。
- **关联**：表示类与类或类与接口之间的依赖关系，表现为“拥有关系”；具体到代码可以用实例变量来表示；
- **聚合**：属于是关联的特殊情况，体现部分-整体关系，是一种弱拥有关系；整体和部分可以有不一样的生命周期；是一种弱关联；
- **组合**：属于是关联的特殊情况，也体现了体现部分-整体关系，是一种强“拥有关系”；整体与部分有相同的生命周期，是一种强关联；
<!-- more -->
Spring IoC容器的依赖有两层含义：`Bean依赖容器`和`容器注入Bean的依赖资源`：

- **Bean依赖容器**：也就是说Bean要依赖于容器，这里的依赖是指容器负责创建Bean并管理Bean的生命周期，正是由于由容器来控制创建Bean并注入依赖，也就是控制权被反转了，这也正是IoC名字的由来，此处的有依赖是指Bean和容器之间的依赖关系。

- **容器注入Bean的依赖资源**：容器负责注入Bean的依赖资源，依赖资源可以是Bean、外部文件、常量数据等，在Java中都反映为对象，并且由容器负责组装Bean之间的依赖关系，此处的依赖是指Bean之间的依赖关系，可以认为是传统类与类之间的“关联”、“聚合”、“组合”关系。
 
为什么要应用依赖注入，应用依赖注入能给我们带来哪些好处呢？

- **动态替换Bean依赖对象，程序更灵活**：替换Bean依赖对象，无需修改源文件：应用依赖注入后，由于可以采用配置文件方式实现，从而能随时动态的替换Bean的依赖对象，无需修改java源文件；
- **更好实践面向接口编程，代码更清晰**：在Bean中只需指定依赖对象的接口，接口定义依赖对象完成的功能，通过容器注入依赖实现；
- **更好实践优先使用对象组合，而不是类继承**：因为IoC容器采用注入依赖，也就是组合对象，从而更好的实践对象组合。

采用对象组合，Bean的功能可能由几个依赖Bean的功能组合而成，其Bean本身可能只提供少许功能或根本无任何功能，全部委托给依赖Bean，对象组合具有动态性，能更方便的替换掉依赖Bean，从而改变Bean功能；
而如果采用类继承，Bean没有依赖Bean，而是采用继承方式添加新功能，，而且功能是在编译时就确定了，不具有动态性，而且采用类继承导致Bean与子Bean之间高度耦合，难以复用。

- **增加Bean可复用性**：依赖于对象组合，Bean更可复用且复用更简单；
- **降低Bean之间耦合**：由于我们完全采用面向接口编程，在代码中没有直接引用Bean依赖实现，全部引用接口，而且不会出现显示的创建依赖对象代码，而且这些依赖是由容器来注入，很容易替换依赖实现类，从而降低Bean与依赖之间耦合；
- **代码结构更清晰**：要应用依赖注入，代码结构要按照规约方式进行书写，从而更好的应用一些最佳实践，因此代码结构更清晰。
 
从以上我们可以看出，其实依赖注入只是一种装配对象的手段，设计的类结构才是基础，如果设计的类结构不支持依赖注入，Spring IoC容器也注入不了任何东西，从而从根本上说“`如何设计好类结构才是关键，依赖注入只是一种装配对象手段`”。

前边IoC一章我们已经了解了Bean依赖容器，那容器如何注入Bean的依赖资源，Spring IoC容器注入依赖资源主要有以下两种基本实现方式：

- **构造器注入**：就是容器实例化Bean时注入那些依赖，通过在在Bean定义中指定构造器参数进行注入依赖，包括实例工厂方法参数注入依赖，但静态工厂方法参数不允许注入依赖；
- **setter注入**：通过setter方法进行注入依赖；
- **方法注入**：能通过配置方式替换掉Bean方法，也就是通过配置改变Bean方法 功能。
 
我们已经知道注入实现方式了，接下来让我们来看看具体配置吧。

##**构造器注入**

使用构造器注入通过配置构造器参数实现，构造器参数就是依赖。除了构造器方式，还有静态工厂、实例工厂方法可以进行构造器注入。如图所示：
![](/img/2013/12/spring3-di-shilihua.jpg)

构造器注入可以根据参数索引注入、参数类型注入或Spring3支持的参数名注入，但参数名注入是有限制的，需要使用在编译程序时打开调试模式（即在编译时使用“`javac –g:vars`”在class文件中生成变量调试信息，默认是不包含变量调试信息的，从而能获取参数名字，否则获取不到参数名字）或在构造器上使用`@ConstructorProperties（java.beans.ConstructorProperties）`注解来指定参数名。

首先让我们准备测试构造器类HelloImpl3.java，该类只有一个包含两个参数的构造器：
{%codeblock lang:java%}    
package cn.javass.spring.chapter3.helloworld;  
public class HelloImpl3 implements HelloApi {  
    private String message;  
private int index;  
//@java.beans.ConstructorProperties({"message", "index"})  
    public HelloImpl3(String message, int index) {  
        this.message = message;  
        this.index = index;  
    }  
    @Override  
    public void sayHello() {  
        System.out.println(index + ":" + message);  
    }  
}  
{%endcodeblock%}

1.根据参数索引注入，使用标签“`<constructor-arg index="1" value="1"/>`”来指定注入的依赖，其中“`index`”表示索引，从0开始，即第一个参数索引为0，“`value`”来指定注入的常量值。
![](/img/2013/12/spring3-di-canshuleixing.jpg)

2.根据参数类型进行注入，使用标签“`<constructor-arg type="java.lang.String" value="Hello World!"/>`”来指定注入的依赖，其中“`type`”表示需要匹配的参数类型，可以是基本类型也可以是其他类型，如“`int`”、“`java.lang.String`”，“`value`”来指定注入的常量值。
![](/img/2013/12/spring3-di-xiandingm.jpg)

3.根据参数名进行注入，使用标签“``<constructor-arg name="message" value="Hello World!"/>`”来指定注入的依赖，其中“`name`”表示需要匹配的参数名字，“`value`”来指定注入的常量值，配置方式如下：
![](/img/2013/12/spring3-canshuming.jpg)

让我们来用具体的例子来看一下构造器注入怎么使用吧。

（1）首先准备Bean类，在此我们就使用“HelloImpl3”这个类。
（2）有了Bean类，接下来要进行Bean定义配置，我们需要配置三个Bean来完成上述三种依赖注入测试，其中Bean ”byIndex”是通过索引注入依赖；Bean ”byType”是根据类型进行注入依赖；Bean ”byName”是根据参数名字进行注入依赖。

{%codeblock lang:xml%} 
<!-- 通过构造器参数索引方式依赖注入 -->  
<bean id="byIndex" class="com.hanfneg.app.dao.HelloImpl3">  
<constructor-arg index="0" value="Hello World!"/>  
    <constructor-arg index="1" value="1"/>  
</bean>  

<!-- 通过构造器参数类型方式依赖注入 -->  
<bean id="byType" class="com.hanfneg.app.dao.HelloImpl3">  
   <constructor-arg type="java.lang.String" value="Hello World!"/>  
   <constructor-arg type="int" value="2"/>  
</bean>  

<!-- 通过构造器参数名称方式依赖注入 -->  
<bean id="byName" class="com.hanfneg.app.dao.HelloImpl3">  
   <constructor-arg name="message" value="Hello World!"/>  
   <constructor-arg name="index" value="3"/>  
</bean> 
{%endcodeblock%}

测试代码：
{%codeblock lang:java%} 
@Test
public void testConstructorDependencyInject(){
	 BeanFactory beanFactory =  
				new ClassPathXmlApplicationContext("classpath:helloworld.xml"); 
	//获取根据参数索引依赖注入的Bean  
	 HelloApi byIndex = beanFactory.getBean("byIndex", HelloApi.class);  
	 byIndex.sayHello();  
	 //获取根据参数类型依赖注入的Bean  
	 HelloApi byType = beanFactory.getBean("byType", HelloApi.class);  
	 byType.sayHello();  
	 //获取根据参数名字依赖注入的Bean  
	 HelloApi byName = beanFactory.getBean("byName", HelloApi.class);  
	 byName.sayHello(); 	 
}
{%endcodeblock%}

通过以上测试我们已经会基本的构造器注入配置了。

##**setter注入**

setter注入，是通过在通过构造器、静态工厂或实例工厂实例好Bean后，通过调用Bean类的setter方法进行注入依赖，如图3-3所示：
![](/img/2013/12/spring3-di-setter1.jpg)

setter注入方式只有一种根据setter名字进行注入：
![](/img/2013/12/spring3-di-setter2.jpg)

通过实例说明;
{%codeblock lang:java%}
package com.hanfneg.app.dao;
public class HelloImpl4 implements HelloApi {  
    private String message;  
    private int index;  
    //setter方法  
    public void setMessage(String message) {  
        this.message = message;  
    }  
    public void setIndex(int index) {  
        this.index = index;  
    }  
    @Override  
    public void sayHello() {  
        System.out.println(index + ":" + message);  
    }  
} 
{%endcodeblock%}

测试代码：都是差不得，大同小异。
{%codeblock lang:java%}
@Test  
	public void testSetterDependencyInject() {  
	    BeanFactory beanFactory =  
	new ClassPathXmlApplicationContext("classpath:helloworld.xml");  
	   HelloApi bean = beanFactory.getBean("bean", HelloApi.class);  
	    bean.sayHello();  
	}
{%endcodeblock%}

知道如何配置了，但Spring如何知道setter方法？如何将值注入进去的呢？其实方法名是要遵守约定的，setter注入的方法名要遵循“`JavaBean getter/setter`方法命名约定”：
 
**JavaBean**：是本质就是一个POJO类，但具有一下限制：
- 该类必须要有公共的无参构造器，如public HelloImpl4() {}；
- 属性为private访问级别，不建议public，如private String message;
- 属性必要时通过一组setter（修改器）和getter（访问器）方法来访问；
- setter方法，以“set” 开头，后跟首字母大写的属性名，如“setMesssage”,简单属性一般只有一个方法参数，方法返回值通常为“void”;
- getter方法，一般属性以“get”开头，对于boolean类型一般以“is”开头，后跟首字母大写的属性名，如“getMesssage”，“isOk”；
- 还有一些其他特殊情况，比如属性有连续两个大写字母开头，如“URL”,则setter/getter方法为：“setURL”和“getURL”，其他一些特殊情况请参看“Java Bean”命名规范。

##**注入集合、数组和字典**

Spring不仅能注入简单类型数据，还能注入集合（Collection、无序集合Set、有序集合List）类型、数组(Array)类型、字典(Map)类型数据、Properties类型数据。

##**引用其它Bean**

上边章节已经介绍了注入常量、集合等基本数据类型和集合数据类型，本小节将介绍注入依赖Bean及注入内部Bean。

引用其他Bean的步骤与注入常量的步骤一样，可以通过构造器注入及setter注入引用其他Bean，只是引用其他Bean的注入配置稍微变化了一下：可以将“`<constructor-arg index="0" value="Hello World!"/>`”和“`<property name="message" value="Hello World!"/>`”中的value属性替换成bean属性，其中bean属性指定配置文件中的其他Bean的id或别名。另一种是把`<value>`标签替换为`<.ref bean=”beanName”>`，bean属性也是指定配置文件中的其他Bean的id或别名。


内容好多 直接看文献吧

##**参考文献**
1.[DI 之 3.1 DI的配置使用](http://jinnianshilongnian.iteye.com/blog/1415277)
