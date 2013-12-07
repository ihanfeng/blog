title: SpringSide4笔记3-QuickStart分析(1)
date: 2013-12-07 15:23:16
categories: SpringSide4
tags: [springside4,quickstart,源码研究]
---
##**项目结构**

  QuickStart项目给予Maven构建的，对于Maven不熟悉的朋友请自行google哈。

##**项目选型**

- Spring Framework 3.2.3
- Hibernate 4.2.3
- Spring data jpa 1.3.2
- sitemesh 2.4.2
- shiro 1.2.2
- hibernate validator 4.3.1

<!-- more -->

##**配置文件分析**

1.pom.xml文件

首先是定义依赖库的版本，独立出来方便管理。

{%codeblock lang:xml%}
<properties>
	<springside.version>4.1.0.GA</springside.version>
	<spring.version>3.2.3.RELEASE</spring.version>
</properties>		
{%endcodeblock%}

依赖项定义

{%codeblock lang:xml%}
<dependencies>
	<!-- SPRINGSIDE -->
	<dependency>
		<groupId>org.springside</groupId>
		<artifactId>springside-core</artifactId>
		<version>${springside.version}</version>
	</dependency>
<dependencies>	
{%endcodeblock%}

其他详情请看项目具体配置。

2.web.xml分析

在web.xml中通过`contextConfigLocation`配置spring，contextConfigLocation参数定义了要装入的 Spring 配置文件。

如果想装入多个配置文件，可以在`<param-value>`标记中用逗号作分隔符

{%codeblock lang:xml%}
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>
		classpath*:/applicationContext.xml,
		classpath*:/applicationContext-shiro.xml
	</param-value>
</context-param>
{%endcodeblock%}

> Spring 3.1的功能，以后就不用为了区分Test, Dev,Production环境，搞几个只有细微区别的application.xml, application-test.xml及引用它们的web.xml了。

首先，将`applicationContext.xml`中的namespace从`3.0`升级到`3.1`.xsd，然后就可以在文件末尾加入不同环境的定义，比如不同的`dataSource`
{%codeblock lang:xml%}
<beans profile="test">
	<jdbc:embedded-database id="dataSource">
		<jdbc:script location="classpath:com/bank/config/sql/schema.sql"/>
	</jdbc:embedded-database>
</beans>
 
<beans profile="production">
	<jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>
</beans>
{%endcodeblock%}
在web.xml里，你需要定义使用的profile，最聪明的做法是定义成`context-param`，注意这里定义的是default值，在非生产环境，可以用系统变量"`spring.profiles.active`"进行覆盖。

{%codeblock lang:xml%}
<context-param>
	<param-name>spring.profiles.default</param-name>
	<param-value>production</param-value>
</context-param>
{%endcodeblock%}

如果您想要在自己所定义的Servlet类别中使用Spring的容器功能，则也可以使用 `org.springframework.web.context.ContextLoaderListener`。ContextLoaderListener预设会读取applicationContext.xml。

{%codeblock lang:xml%}
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
{%endcodeblock%}

加入encodingFilter解决中文乱码问题。`<filter-mapping>`紧紧的放在`<filter>`的后面该配置顺序不可调换，切记，不然无效哦。

{%codeblock lang:xml%}
<filter>
	<filter-name>encodingFilter</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>UTF-8</param-value>
	</init-param>
	<init-param>
		<param-name>forceEncoding</param-name>
		<param-value>true</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>encodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
{%endcodeblock%}

在Java Web项目中使用`Hibernate`经常会遇到`LazyInitializationException`。这是因为controller和model层（java代码）将通过JPA的一些启用了延迟加载功能 的领域（如用`getRefrence()` 方法或者在关联关系中采用`fetch=FetchType.LAZY` ）返回给view层（jsp代码）的时候，由于加载领域对象的`JPA Session`已经关闭，导致这些延迟加载的数据访问异常。

这时就可以使用OpenEntityManagerInViewFilter来将一个JPAsession与一次完整的请求过程对应的线程相绑定。

{%codeblock lang:xml%}
<filter>
	<filter-name>openEntityManagerInViewFilter</filter-name>
	<filter-class>org.springframework.orm.jpa.support.OpenEntityManagerInViewFilter</filter-class>
</filter>
<filter-mapping>
	<filter-name>openEntityManagerInViewFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
{%endcodeblock%}

配置Shiro过滤器,先让Shiro过滤系统接收到的请求。这里`filter-name`必须对应`applicationContext.xml`中定义的`<bean id="shiroFilter"/>`。使用`[/*]`匹配所有请求,保证所有的可控请求都经过Shiro的过滤。
通常会将此`filter-mapping`放置到最前面(即其他filter-mapping前面),以保证它是过滤器链中第一个起作用的

{%codeblock lang:xml%}
<filter>
	<filter-name>shiroFilter</filter-name>
	<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	<init-param>
	 <!-- 该值缺省为false,表示生命周期由SpringApplicationContext管理,
	      设置为true则表示由ServletContainer管理 -->  
		<param-name>targetFilterLifecycle</param-name>
		<param-value>true</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>shiroFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
{%endcodeblock%}

配置SiteMeshFilter，否则SiteMesh不起作用哦，什么是SiteMesh，请自行google。

{%codeblock lang:xml%}
<filter>
	<filter-name>sitemeshFilter</filter-name>
	<filter-class>com.opensymphony.sitemesh.webapp.SiteMeshFilter</filter-class>
</filter>
<filter-mapping>
	<filter-name>sitemeshFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
{%endcodeblock%}

`DispatcherServlet`是前端控制器设计模式的实现，提供Spring Web MVC的集中访问点，而且负责职责的分派，而且与Spring IoC容器无缝集成。

{%codeblock lang:xml%}
<servlet>
	<servlet-name>springServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<!--加载spring-mvc.xml进行初始化-->
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring-mvc.xml</param-value>
	</init-param>
	 <!--启动时初始化servlet-->
	<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
	<servlet-name>springServlet</servlet-name>
	<!--拦截所有的请求-->
	<url-pattern>/</url-pattern>
</servlet-mapping>
{%endcodeblock%}

设置session会话时间

{%codeblock lang:xml%}
<session-config>
	<session-timeout>20</session-timeout>
</session-config>
{%endcodeblock%}

配置系统错误页面

{%codeblock lang:xml%}
<error-page>
	<exception-type>java.lang.Throwable</exception-type>
	<location>/WEB-INF/views/error/500.jsp</location>
</error-page>
<error-page>
	<error-code>500</error-code>
	<location>/WEB-INF/views/error/500.jsp</location>
</error-page>
<error-page>
	<error-code>404</error-code>
	<location>/WEB-INF/views/error/404.jsp</location>
</error-page>
{%endcodeblock%}


##**参考文献**
1.[用Profile 统一环境配置](https://github.com/springside/springside4/wiki/Spring)
2.[org.springframework.web.context.ContextLoaderListener作用](http://blog.csdn.net/taijianyu/article/details/3176263)
3.[Spring的OpenEntityManagerInViewFilter](http://whoosh.iteye.com/blog/1300721)
4.[SpringMVC整合Shiro](http://blog.csdn.net/jadyer/article/details/12208847)
5.[DispatcherServlet详解](http://jinnianshilongnian.iteye.com/blog/1602617)
