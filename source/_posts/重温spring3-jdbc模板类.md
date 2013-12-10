title: 重温spring3-JDBC模板类
date: 2013-12-10 09:55:19
categories: Spring3
tags: [Spring3,JDBC,模板类]
---
##**概述**

`Spring JDBC`抽象框架core包提供了JDBC模板类，其中`JdbcTemplate`是core包的核心类，所以其他模板类都是基于它封装完成的，JDBC模板类是第一种工作模式。
 
JdbcTemplate类通过模板设计模式帮助我们消除了冗长的代码，只做需要做的事情（即可变部分），并且帮我们做哪些固定部分，如连接的创建及关闭。
 <!-- more -->
JdbcTemplate类对可变部分采用回调接口方式实现，如`ConnectionCallback`通过回调接口返回给用户一个连接，从而可以使用该连接做任何事情、`StatementCallback`通过回调接口返回给用户一个Statement，从而可以使用该Statement做任何事情等等，还有其他一些回调接口如图7-3所示。
![](/img/2013/12/spring3-jdbc-mubanlei.jpg)

Spring除了提供JdbcTemplate核心类，还提供了基于JdbcTemplate实现的`NamedParameterJdbcTemplate`类用于支持命名参数绑定、 `SimpleJdbcTemplate`类用于支持Java5+的可变参数及自动装箱拆箱等特性。

##**传统JDBC编程替代方案**

前边我们已经使用过传统JDBC编程方式，接下来让我们看下Spring JDBC框架提供的更好的解决方案。

在使用JdbcTemplate模板类时必须通过DataSource获取数据库连接，Spring JDBC提供了DriverManagerDataSource实现，它通过包装“`DriverManager.getConnection`”获取数据库连接，