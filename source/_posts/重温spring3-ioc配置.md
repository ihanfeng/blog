title: 重温spring3-IoC配置
date: 2013-12-09 10:44:02
categories: Spring3
tags: [Spring3,IoC配置]
---
##**XML配置的结构**

一般配置结构如下：

{%codeblock lang:xml%}
<beans>  
    <import resource=”resource1.xml”/>  
    <bean id=”bean1”class=””></bean>  
    <bean id=”bean2”class=””></bean>  
<bean name=”bean2”class=””></bean>  
    <alias alias="bean3" name="bean2"/>  
    <import resource=”resource2.xml”/>  
</beans>  
{%endcodeblock%}
<!-- more -->
说明：

1.`<bean>`标签主要用来进行Bean定义；
2.`alias`用于定义Bean别名的；
3.`import`用于导入其他配置文件的Bean定义，这是为了加载多个配置文件，当然也可以把这些配置文件构造为一个数组`(new String[] {“config1.xml”, config2.xml})`传给ApplicationContext实现进行加载多个配置文件，那一个更适合由用户决定；这两种方式都是通过调用Bean Definition Reader 读取Bean定义，内部实现没有任何区别。`<import>`标签可以放在`<beans>`下的任何位置，没有顺序关系。

##**Bean的配置**

Spring IoC容器目的就是管理Bean，这些Bean将根据配置文件中的Bean定义进行创建，而Bean定义在容器内部由BeanDefinition对象表示，该定义主要包含以下信息：

- 全限定类名（FQN）：用于定义Bean的实现类;
- Bean行为定义：这些定义了Bean在容器中的行为；包括作用域（单例、原型创建）、是否惰性初始化及生命周期等；
- Bean创建方式定义：说明是通过构造器还是工厂方法创建Bean；
- Bean之间关系定义：即对其他bean的引用，也就是依赖关系定义，这些引用bean也可以称之为同事bean 或依赖bean，也就是依赖注入。

Bean定义只有“全限定类名”在当使用构造器或静态工厂方法进行实例化bean时是必须的，其他都是可选的定义。难道Spring只能通过配置方式来创建Bean吗？回答当然不是，某些SingletonBeanRegistry接口实现类实现也允许将那些非BeanFactory创建的、已有的用户对象注册到容器中，这些对象必须是共享的，比如使用DefaultListableBeanFactory 的registerSingleton() 方法。不过建议采用元数据定义。

##**Bean的命名**

每个Bean可以有一个或多个id（或称之为标识符或名字），在这里我们把第一个id称为“标识符”，其余id叫做“别名”；这些id在IoC容器中`必须唯一`。如何为Bean指定id呢，有以下几种方式；

1.不指定id，只配置必须的`全限定类名`，由IoC容器为其`生成`一个标识，客户端必须通过接口“T getBean(Class<T> requiredType)”获取Bean；

{%codeblock lang:xml%}
<bean class="com.hanfneg.app.dao.HelloApiImpl"></bean>  
{%endcodeblock%}

测试代码：
{%codeblock lang:java%}
@Test
public void testHelloWord1(){
	BeanFactory beanFactory =  
			   new ClassPathXmlApplicationContext("classpath:helloworld.xml");  
	// 根据类型获取bean
	HelloApi helloApi = beanFactory.getBean(HelloApi.class);
	helloApi.sayHello();
}
{%endcodeblock%}
或者
{%codeblock lang:java%}
@Test
public void testHelloWord1(){
	ApplicationContext context = new ClassPathXmlApplicationContext("classpath:helloworld.xml");
	// 根据类型获取bean
	HelloApi helloApi = context.getBean(HelloApi.class);
	helloApi.sayHello();
}
{%endcodeblock%}

2.指定id，必须在Ioc容器中唯一；

代码请看helloworld实例。

3.指定name，这样name就是“标识符”，必须在Ioc容器中唯一。

{%codeblock lang:xml%}
<bean name="hello" class="com.hanfneg.app.dao.HelloApiImpl"></bean>  
{%endcodeblock%}

测试代码和指定id一样。

4.指定id和name，id就是标识符，而name就是别名，必须在Ioc容器中唯一。

{%codeblock lang:xml%}
<bean id="hello1" name="hello2" class="com.hanfneg.app.dao.HelloApiImpl"></bean>  
<!-- 如果id和name一样，IoC容器能检测到，并消除冲突 -->  
<bean id="hello" name="hello" class="com.hanfneg.app.dao.HelloApiImpl"></bean>  
{%endcodeblock%}


5.指定多个name，多个name用“，”、“；”、“ ”分割，第一个被用作标识符，其他的（alias1、alias2、alias3）是别名，所有标识符也必须在Ioc容器中唯一；
{%codeblock lang:xml%}
<bean name=” bean1;alias11,alias12;alias13 alias14”  
      class=” cn.javass.spring.chapter2.helloworld.HelloImpl”/>     
<!-- 当指定id时，name指定的标识符全部为别名 -->  
<bean id="bean2" name="alias21;alias22"  
class="cn.javass.spring.chapter2.helloworld.HelloImpl"/>         
{%endcodeblock%}

6.使用<alias>标签指定别名，别名也必须在IoC容器中唯一
{%codeblock lang:xml%}
<bean name="bean" class="cn.javass.spring.chapter2.helloworld.HelloImpl"/>  
<alias alias="alias1" name="bean"/>  
<alias alias="alias2" name="bean"/>  
{%endcodeblock%}     

从定义来看，name或id如果指定它们中的一个时都作为“标识符”，那为什么还要有id和name同时存在呢？这是因为当使用基于XML的配置元数据时，在XML中id是一个真正的XML id属性，因此当其他的定义来引用这个id时就体现出id的好处了，可以利用XML解析器来验证引用的这个id是否存在，从而更早的发现是否引用了一个不存在的bean，而使用name，则可能要在真正使用bean时才能发现引用一个不存在的bean。
 
Bean命名约定：Bean的命名遵循XML命名规范，但最好符合Java命名规范，由“字母、数字、下划线组成“，而且应该养成一个良好的命名习惯， 比如采用“驼峰式”，即第一个单词首字母开始，从第二个单词开始首字母大写开始，这样可以增加可读性。

##**实例化Bean**

Spring IoC容器如何实例化Bean呢？传统应用程序可以通过new和反射方式进行实例化Bean。而Spring IoC容器则需要根据Bean定义里的配置元数据使用反射机制来创建Bean。在Spring IoC容器中根据Bean定义创建Bean主要有以下几种方式：

1.**使用构造器实例化Bean**：这是最简单的方式，`Spring IoC`容器即能使用默认空构造器也能使用有参数构造器两种方式创建Bean，如以下方式指定要创建的Bean类型：
 
使用空构造器进行定义，使用此种方式，class属性指定的类必须有空构造器
{%codeblock lang:xml%}
<bean name="bean1" class="cn.javass.spring.chapter2.HelloImpl2"/>  
{%endcodeblock%}  
使用有参数构造器进行定义，使用此中方式，可以使用< constructor-arg >标签指定构造器参数值，其中index表示位置，value表示常量值，也可以指定引用，指定引用使用ref来引用另一个Bean定义，后边会详细介绍：
{%codeblock lang:xml%}
<bean name="bean2" class="cn.javass.spring.chapter2.HelloImpl2">  
<!-- 指定构造器参数 -->  
     <constructor-arg index="0" value="Hello Spring!"/>  
</bean>  
{%endcodeblock%}  

知道如何配置了，让我们做个例子的例子来实践一下吧：
（1）准备Bean class(HelloImpl2.java)，该类有一个空构造器和一个有参构造器：
{%codeblock lang:java%}
package com.hanfneg.app.dao;

public class HelloImpl2 implements HelloApi {

	private String message;

	//无参构造函数
	public HelloImpl2() {
		this.message = "Hello";
	}

	//有参构造函数
	public HelloImpl2(String message) {
		this.message = message;
	}

	@Override
	public void sayHello() {
		System.out.println(message);  
	}

}
{%endcodeblock%} 

在配置文件(resources/helloword.xml)配置Bean定义，如下所示：

{%codeblock lang:xml%}
<!--使用默认构造参数-->  
<bean name="bean1" class="com.hanfneg.app.dao.HelloImpl2"/>  

<!--使用有参数构造参数-->     
<bean name="bean2" class="com.hanfneg.app.dao.HelloImpl2">  
	<!-- 指定构造器参数 -->  
    <constructor-arg index="0" value="Hello Spring!"/>  
</bean>  
{%endcodeblock%} 

配置完了，让我们写段测试代码（InstantiatingContainerTest）来看下是否工作吧：

{%codeblock lang:java%}
@Test
public void testHelloWord2(){
	ApplicationContext context = new ClassPathXmlApplicationContext("classpath:helloworld.xml");
	
	HelloApi helloApi1 = context.getBean("bean1",HelloApi.class);
	helloApi1.sayHello();
	HelloApi helloApi2 = context.getBean("bean2",HelloApi.class);
	helloApi2.sayHello();
}
{%endcodeblock%} 

输出结果：

	2013-12-9 11:34:37 org.springframework.context.support.AbstractApplicationContext prepareRefresh
	信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@1fe88d: startup date [Mon Dec 09 11:34:37 CST 2013]; root of context hierarchy
	2013-12-9 11:34:37 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
	信息: Loading XML bean definitions from class path resource [helloworld.xml]
	2013-12-9 11:34:38 org.springframework.beans.factory.support.DefaultListableBeanFactory preInstantiateSingletons
	信息: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@1d709a5: defining beans [hello,bean1,bean2]; root of factory hierarchy
	Hello
	Hello Spring!


2.使用`静态工厂`方式实例化Bean，使用这种方式除了指定必须的class属性，还要指定factory-method属性来指定实例化Bean的方法，而且使用静态工厂方法也允许指定方法参数，spring IoC容器将调用此属性指定的方法来获取Bean，配置如下所示：
{%codeblock lang:java%}
package com.hanfneg.app.dao;
public class HelloApiStaticFactory {  
    //工厂方法  
       public static HelloApi newInstance(String message) {  
              //返回需要的Bean实例  
           return new HelloImpl2(message);  
}  
} 
{%endcodeblock%} 

静态工厂写完了，配置Bean定义：

{%codeblock lang:xml%}
<!-- 使用静态工厂方法 -->  
<bean id="bean3" class="com.hanfneg.app.dao.HelloApiStaticFactory" factory-method="newInstance">  
     <constructor-arg index="0" value="Hello Spring!"/>  
</bean> 
{%endcodeblock%} 
配置完了，写段测试代码来测试一下吧，InstantiatingBeanTest：
{%codeblock lang:java%}
@Test  
public void testInstantiatingBeanByStaticFactory() {  
       //使用静态工厂方法  
       BeanFactory beanFactory =  
new ClassPathXmlApplicationContext("classpath:helloworld.xml");  
       HelloApi bean3 = beanFactory.getBean("bean3", HelloApi.class);  
       bean3.sayHello();  
} 
{%endcodeblock%} 


3.使用实例工厂方法实例化Bean，使用这种方式不能指定class属性，此时必须使用factory-bean属性来指定工厂Bean，factory-method属性指定实例化Bean的方法，而且使用实例工厂方法允许指定方法参数，方式和使用构造器方式一样，配置如下：

实例工厂类代码（HelloApiInstanceFactory.java）如下：
{%codeblock lang:java%}    
package cn.javass.spring.chapter2;  
public class HelloApiInstanceFactory {  
public HelloApi newInstance(String message) {  
          return new HelloImpl2(message);  
   }  
}  
{%endcodeblock%}    

让我们在配置文件(helloworld.xml)配置Bean定义：
 
{%codeblock lang:xml%}  
<!—1、定义实例工厂Bean -->  
<bean id="beanInstanceFactory"  
class="com.hanfneg.app.dao.HelloApiInstanceFactory"/>  
<!—2、使用实例工厂Bean创建Bean -->  
<bean id="bean4"  
factory-bean="beanInstanceFactory"  
     factory-method="newInstance">  
 <constructor-arg index="0" value="Hello Spring!"></constructor-arg>  
</bean>  
{%endcodeblock%} 

测试代码InstantiatingBeanTest：
 
{%codeblock lang:java%}    
@Test  
public void testInstantiatingBeanByInstanceFactory() {  
//使用实例工厂方法  
       BeanFactory beanFactory =  
new ClassPathXmlApplicationContext("classpath:helloworld.xml");  
       HelloApi bean4 = beanFactory.getBean("bean4", HelloApi.class);  
       bean4.sayHello();  
}  
{%endcodeblock%}  

通过以上例子我们已经基本掌握了如何实例化Bean了，大家是否注意到？这三种方式只是配置不一样，从获取方式看完全一样，没有任何不同。这也是Spring IoC的魅力，Spring IoC帮你创建Bean，我们只管使用就可以了，是不是很简单。

##**小结**

到此我们已经讲完了Spring IoC基础部分，包括IoC容器概念，如何实例化容器，Bean配置、命名及实例化，Bean获取等等。不知大家是否注意到到目前为止，我们只能通过简单的实例化Bean，没有涉及Bean之间关系。接下来一章让我们进入配置Bean之间关系章节，也就是依赖注入。
