title: springside4学习笔记1-SpringSide4介绍及环境配置
date: 2013-12-07 08:44:34
categories: SpringSide4
tags: [框架,spring,springside,江南白衣]
---
##*介绍*

`SpringSide`是以`Spring Framework`为核心的，`Pragmatic`风格的`JavaEE`应用参考示例，是JavaEE世界中的主流技术选型，最佳实践的总结与演示。

- Quickstart: 一个迷你的TodoList应用。
- Showcase: 五花八门的JavaEE技术大派对。

<!-- more -->

官网：[http://www.springside.org.cn/](http://www.springside.org.cn/)
Github:[https://github.com/springside/springside4](https://github.com/springside/springside4)
讨论区：[http://www.oschina.net/p/springside](http://www.oschina.net/p/springside)

##**入门研究**

###**系统环境**

必须保证满足下列2个条件：

1.Install JDK 6.0+ and set the JAVA_HOME.
2.Install Maven 3.0.3+ and set the PATH. 

###**SpringSide4目录**
1.`Modules` -- SpringSide封装的代码: 

  - `Parent`是公共的pom.xml文件；
  - `Core`是一些使用率最高的核心代码；
  - `Extension`是不一定会用上的扩展如Memcached/Redis Client封装；
  - `Test` 则是测试用的封装；

2.`Examples` -- QuickStart 与 Showcase 一小一大两个示例项目.
3.`Support` -- 其他杂项内容, 如H2的Console启动命令, Maven的常用命令, 生成新项目的模板，Sonar的规则等等.

###**实例运行**

默认情况下，我们直接执行根目录下下的`quick-start.bat`就可以全自动的运行项目。

下面我们使用手工的方式自己输入命令，然后导入项目到eclipse。

1.将所有module编译打包安装到Maven的本地仓库：进入`E:\git_2\springside4\modules`目录（以你自己的项目目录为准，这是我的路径）然后执行：

	mvn install

![](/img/2013/12/springside4-module-mvn-install.jpg)


2.将`quickstart`项目生成为普通elipse项目，这样就可以在eclipse中导入(import)了。进入`E:\git_2\springside4\examples\quickstart`,执行命令:

	mvn eclipse:eclipse

注：其实可以忽略第2步的，我们在eclipse中直接导入maven项目即可，前提是你eclipse安装maven插件。

![](/img/2013/12/springside4-example-import.jpg)

3.配置数据库，默认使用的是H2 databse，我更改为mysql，方便研究调试，你可以按自己的需要来。

修改pom.xml文件，注释掉h2的相关配置，更改为mysql。
{%codeblock lang:xml%}
<!--  <h2.version>1.3.174</h2.version>-->

<!-- Plugin的属性定义 -->
<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
<jdk.version>1.6</jdk.version>

<!-- 项目属性 -->
<!--
<jdbc.driver.groupId>com.h2database</jdbc.driver.groupId>
<jdbc.driver.artifactId>h2</jdbc.driver.artifactId>
<jdbc.driver.version>${h2.version}</jdbc.driver.version>
-->	
<jdbc.driver.groupId>mysql</jdbc.driver.groupId>
<jdbc.driver.artifactId>mysql-connector-java</jdbc.driver.artifactId>
<jdbc.driver.version>5.1.22</jdbc.driver.version>
{%endcodeblock%}

修改刷新开发环境

{%codeblock lang:xml%}
<sql driver="${jdbc.driver}" url="${jdbc.url}" userid="${jdbc.username}" password="${jdbc.password}" onerror="continue" encoding="${project.build.sourceEncoding}">
	<classpath refid="maven.test.classpath" />
	<transaction src="src/main/resources/sql/mysql/schema.sql"/>
	<transaction src="src/test/resources/data/h2/import-data.sql"/>
</sql>
{%endcodeblock%}

修改application.properties

{%codeblock lang:xml%}
#mysql database setting
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost/quickstart?useUnicode=true&characterEncoding=utf-8
jdbc.username=root
jdbc.password=123456
{%endcodeblock%}

创建数据表quickstart，运行refresh-db.bat刷新数据库。

###**运行quickstart**

使用内嵌的jetty server运行项目
在eclipse中，右击quickstart中的pom.xml文件
选择：Run Configuration

Base diretory： 选择quickstart项目
Goals: jetty:run

点击run，运行该项目。

或部署到tomcat上面。我是使用tomcat，部署成功如下图：
![](/img/2013/12/springside4-quickstart-demo.jpg)

登陆下例子看看如何：
![](/img/2013/12/springside4-quickstart-demo2.jpg)

下面就可以开始根据例子对springside的核心代码研究。请看后续博文。
