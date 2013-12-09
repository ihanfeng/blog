title: 重温spring3-AOP的HelloWorld
date: 2013-12-09 16:30:52
categories: Spring3
tags: [Spring,AOP]
---
##**定义目标类**

定义目标接口：

{%codeblock lang:java%}
package com.hanfeng.app.service;
public interface IHelloWorldService {  
    public void sayHello();  
}  
{%endcodeblock%}
<!-- more -->
实现接口类：
{%codeblock lang:java%}
package com.hanfeng.app.service;
public class HelloWorldService implements IHelloWorldService {  
    @Override  
    public void sayHello() {  
        System.out.println("============Hello World!");  
    }  
} 
{%endcodeblock%}

  注：在日常开发中最后将业务逻辑定义在一个专门的service包下，而实现定义在service包下的impl包中，服务接口以IXXXService形式，而服务实现就是XXXService，这就是规约设计，见名知义。当然可以使用公司内部更好的形式，只要大家都好理解就可以了。

##**定义切面支持类**

有了目标类，该定义切面了，切面就是通知和切入点的组合，而切面是通过配置方式定义的，因此这定义切面前，我们需要定义切面支持类，切面支持类提供了通知实现：
{%codeblock lang:java%}
package com.hanfeng.app.aop;

public class HelloWorldAspect {
	// 前置通知
	public void beforeAdvice() {
		System.out.println("===========before advice");
	}

	// 后置最终通知
	public void afterFinallyAdvice() {
		System.out.println("===========after finally advice");
	}
}
{%endcodeblock%}

##**在XML中进行配置**

有了通知实现，那就让我们来配置切面吧：

1. 配置目标类：

{%codeblock lang:xml%}
<bean id="helloWorldService" class="com.hanfeng.app.service.HelloWorldService" />
{%endcodeblock%}

2.配置切面
{%codeblock lang:xml%}
<bean id="aspect" class="com.hanfeng.app.aop.HelloWorldAspect"/>  
<aop:config>  
<aop:pointcut id="pointcut" expression="execution(* com.hanfeng..*.*(..))"/>  
    <aop:aspect ref="aspect">  
        <aop:before pointcut-ref="pointcut" method="beforeAdvice"/>  
        <aop:after pointcut="execution(* com.hanfeng..*.*(..))" method="afterFinallyAdvice"/>  
    </aop:aspect>  
</aop:config>  
{%endcodeblock%}

切入点使用`<aop:config>`标签下的`<aop:pointcut>`配置，`expression`属性用于定义切入点模式，默认是AspectJ语法，“`execution(* cn.javass..*.*(..))`”表示匹配`com.hanfeng`包及子包下的任何方法执行。
 
切面使用`<aop:config>`标签下的`<aop:aspect>`标签配置，其中“`ref`”用来引用切面支持类的方法。
 
前置通知使用`<aop:aspect>`标签下的`<aop:before>`标签来定义，`pointcut-ref`属性用于引用切入点`Bean`，而`method`用来引用切面通知实现类中的方法，该方法就是通知实现，即在目标类方法执行之前调用的方法。
 
最终通知使用`<aop:aspect>`标签下的`<aop:after >`标签来定义，切入点除了使用`pointcut-ref`属性来引用已经存在的切入点，也可以使用`pointcut`属性来定义，如`pointcut="execution(* com.hanfeng..*.*(..))`"，`method`属性同样是指定通知实现，即在目标类方法执行之后调用的方法。

3.测试程序

测试类非常简单，调用被代理Bean跟调用普通Bean完全一样，Spring AOP将为目标对象创建AOP代理，具体测试代码如下：

{%codeblock lang:java%}
@Test
public void testHelloworld() {
	ApplicationContext ctx = new ClassPathXmlApplicationContext(
			"classpath:helloworld.xml");
	IHelloWorldService helloworldService = ctx.getBean("helloWorldService",
			IHelloWorldService.class);
	helloworldService.sayHello();
}
{%endcodeblock%}

该测试将输出如下如下内容：

	2013-12-9 16:45:45 org.springframework.context.support.AbstractApplicationContext prepareRefresh
	信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@cf2c80: startup date [Mon Dec 09 16:45:45 CST 2013]; root of context hierarchy
	2013-12-9 16:45:45 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
	信息: Loading XML bean definitions from class path resource [helloworld.xml]
	2013-12-9 16:45:46 org.springframework.beans.factory.support.DefaultListableBeanFactory preInstantiateSingletons
	信息: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@1342ba4: defining beans [hello,bean1,bean2,bean3,byIndex,byType,byName,bean,helloWorldService,aspect,org.springframework.aop.config.internalAutoProxyCreator,pointcut,org.springframework.aop.aspectj.AspectJPointcutAdvisor#0,org.springframework.aop.aspectj.AspectJPointcutAdvisor#1]; root of factory hierarchy
	===========before advice
	============Hello World!
	===========after finally advice

从输出我们可以看出：前置通知在切入点选择的连接点（方法）之前允许，而后置通知将在连接点（方法）之后执行，具体生成AOP代理及执行过程如图6-4所示。
![](/img/2013/12/spring3-aop-guocheng.jpg)

##**参考文献**
1.[AOP 之 6.2 AOP的HelloWorld](http://jinnianshilongnian.iteye.com/blog/1418597)
