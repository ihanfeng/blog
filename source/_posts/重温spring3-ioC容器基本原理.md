title: 重温spring3-IoC容器基本原理
date: 2013-12-09 09:51:47
categories: Spring3
tags: [spring3,IoC容器]
---
##**IoC容器的概念**

`IoC容器`就是具有`依赖注入`功能的容器，IoC容器负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。应用程序无需直接在代码中new相关的对象，应用程序由IoC容器进行组装。在Spring中BeanFactory是IoC容器的实际代表者。

Spring IoC容器如何知道哪些是它管理的对象呢？这就需要配置文件，Spring IoC容器通过读取配置文件中的配置元数据，通过元数据对应用中的各个对象进行实例化及装配。一般使用基于xml配置文件进行配置元数据，而且Spring与配置文件完全解耦的，可以使用其他任何可能的方式进行配置元数据，比如注解、基于java文件的、基于属性文件的配置都可以。
<!-- more -->
那Spring IoC容器管理的对象叫什么呢？

##**Bean的概念**

由IoC容器管理的那些组成你应用程序的对象我们就叫它`Bean`， Bean就是*由Spring容器初始化、装配及管理的对象*，除此之外，bean就与应用程序中的其他对象没有什么区别了。那IoC怎样确定如何实例化Bean、管理Bean之间的依赖关系以及管理Bean呢？这就需要配置元数据，在Spring中由BeanDefinition代表，后边会详细介绍，配置元数据指定如何实例化Bean、如何组装Bean等。概念知道的差不多了，让我们来做个简单的例子。

##**Hello World实例**

本实例基于Maven创建。

配置pom.xml，导入spring所需的jar。通过spring-core、springwebmvc、spring-orm即可获得12个spring包。

{%codeblock lang:xml%}
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-core</artifactId>
	<version>3.2.5.RELEASE</version>
</dependency>
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-webmvc</artifactId>
	<version>3.2.5.RELEASE</version>
</dependency>
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-orm</artifactId>
	<version>3.2.5.RELEASE</version>
</dependency>  
{%endcodeblock%}

![](/img/2013/12/spring3-hello-jar.jpg)

项目框架搭建完毕，开始编写代码。

首先定义一个"sayHello"的接口

{%codeblock lang:java%}
package com.hanfneg.app.dao;

public interface HelloApi {
	public void sayHello();
}
{%endcodeblock%}

实现接口的方法，输出打印信息。

{%codeblock lang:java%}
package com.hanfneg.app.dao;

public class HelloApiImpl implements HelloApi{

	@Override
	public void sayHello() {
		System.out.println("你好，我在学习IoC");
	}

}
{%endcodeblock%}

接口和实现都开发好了，那如何使用Spring IoC容器来管理它们呢？这就需要配置文件，让IoC容器知道要管理哪些对象。让我们来看下配置文件helloworld.xml（放到src/main/resources目录下）：

{%codeblock lang:xml%}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:jdbc="http://www.springframework.org/schema/jdbc"  
	xmlns:jee="http://www.springframework.org/schema/jee" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:jpa="http://www.springframework.org/schema/data/jpa"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd
		http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-3.2.xsd
		http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-3.2.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.2.xsd
		http://www.springframework.org/schema/data/jpa http://www.springframework.org/schema/data/jpa/spring-jpa-1.3.xsd"
		>
		
	<!-- id 表示你这个组件的名字，class表示组件类 -->  
	<bean id="hello" class="com.hanfneg.app.dao.HelloApiImpl"></bean>  
		
</beans>
{%endcodeblock%}

现在万一具备，那如何获取IoC容器并完成我们需要的功能呢？首先应该实例化一个IoC容器，然后从容器中获取需要的对象，然后调用接口完成我们需要的功能，代码示例如下：

{%codeblock lang:java%}
package com.hanfneg.app.test;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.hanfneg.app.dao.HelloApi;

public class HelloTest {
	
	@Test
	public void testHelloWorld(){
		 //1、读取配置文件实例化一个IoC容器  
		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:helloworld.xml");
		 //2、从容器中获取Bean，注意此处完全“面向接口编程，而不是面向实现”  
		HelloApi helloApi = context.getBean("hello",HelloApi.class);
		 //3、执行业务逻辑  
		helloApi.sayHello();
	}
}

{%endcodeblock%}

执行测试代码结果：

	2013-12-9 10:20:26 org.springframework.context.support.AbstractApplicationContext prepareRefresh
	信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@148662: startup date [Mon Dec 09 10:20:26 CST 2013]; root of context hierarchy
	2013-12-9 10:20:26 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
	信息: Loading XML bean definitions from class path resource [helloworld.xml]
	2013-12-9 10:20:27 org.springframework.beans.factory.support.DefaultListableBeanFactory preInstantiateSingletons
	信息: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@126c6ea: defining beans [hello]; root of factory hierarchy
	你好，我在学习IoC

说明程序是正确的。

自此一个完整的Spring Hello World已完成，是不是很简单，让我们深入理解下容器和Bean吧。

##**详解IoC容器**

在Spring Ioc容器的代表就是`org.springframework.beans`包中的`BeanFactory`接口，BeanFactory接口提供了IoC容器最基本功能；而`org.springframework.context`包下的`ApplicationContext`接口扩展了BeanFactory，还提供了与`Spring AOP集成`、`国际化处理`、`事件传播`及`提供不同层次的context实现`(如针对web应用的WebApplicationContext)。简单说， BeanFactory提供了IoC容器最基本功能，而 ApplicationContext 则增加了更多支持企业级功能支持。ApplicationContext完全继承BeanFactory，因而BeanFactory所具有的语义也适用于ApplicationContext。

容器实现一览：

- **XmlBeanFactory**：BeanFactory实现，提供基本的IoC容器功能，可以从classpath或文件系统等获取资源；
{%codeblock lang:java%}
File file = new File("fileSystemConfig.xml");
Resource resource = new FileSystemResource(file);
BeanFactory beanFactory = new XmlBeanFactory(resource);

Resource resource = new ClassPathResource("classpath.xml");                 
BeanFactory beanFactory = new XmlBeanFactory(resource);
{%endcodeblock%}

- **ClassPathXmlApplicationContext**：ApplicationContext实现，从classpath获取配置文件；
{%codeblock lang:java%}
BeanFactory beanFactory = new ClassPathXmlApplicationContext("classpath.xml");
{%endcodeblock%}

- **FileSystemXmlApplicationContext**：ApplicationContext实现，从文件系统获取配置文件。
{%codeblock lang:java%}
BeanFactory beanFactory = new FileSystemXmlApplicationContext("fileSystemConfig.xml");
{%endcodeblock%}

ApplicationContext接口获取Bean方法简介：

- `Object getBean(String name)` 根据名称返回一个Bean，客户端需要自己进行类型转换；
- `T getBean(String name, Class<T> requiredType)` 根据名称和指定的类型返回一个Bean，客户端无需自己进行类型转换，如果类型转换失败，容器抛出异常；
- `T getBean(Class<T> requiredType)` 根据指定的类型返回一个Bean，客户端无需自己进行类型转换，如果没有或有多于一个Bean存在容器将抛出异常；
- `Map<String, T> getBeansOfType(Class<T> type)` 根据指定的类型返回一个键值为名字和值为Bean对象的 Map，如果没有Bean对象存在则返回空的Map。

让我们来看下IoC容器到底是如何工作。在此我们以xml配置方式来分析一下：

1.准备配置文件：就像前边Hello World配置文件一样，在配置文件中声明Bean定义也就是为Bean配置元数据。
2.由IoC容器进行解析元数据： IoC容器的Bean Reader读取并解析配置文件，根据定义生成BeanDefinition配置元数据对象，IoC容器根据BeanDefinition进行实例化、配置及组装Bean。
3.实例化IoC容器：由客户端实例化容器，获取需要的Bean。
 
整个过程是不是很简单，执行过程如下图，其实IoC容器很容易使用，主要是如何进行Bean定义。下一章我们详细介绍定义Bean。

![](/img/2013/12/spring3-iocrongqi.jpg)


##**小结**

除了测试程序的代码外，也就是程序入口，所有代码都没有出现Spring任何组件，而且所有我们写的代码没有实现框架拥有的接口，因而能非常容易的替换掉Spring，是不是非入侵。
客户端代码完全面向接口编程，无需知道实现类，可以通过修改配置文件来更换接口实现，客户端代码不需要任何修改。是不是低耦合。
如果在开发初期没有真正的实现，我们可以模拟一个实现来测试，不耦合代码，是不是很方便测试。
Bean之间几乎没有依赖关系，是不是很容易重用。



