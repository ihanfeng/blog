title: JSR 303 - Bean Validation 介绍
date: 2013-12-07 11:54:28
categories: Java进阶
tags: [JSR 303,Bean Validation]
---
##**简介**

`JSR 303 – Bean Validation` 是一个数据验证的规范，2009 年 11 月确定最终方案。2009 年 12 月 Java EE 6 发布，Bean Validation 作为一个重要特性被包含其中。本文将对 `Bean Validation` 的主要功能进行介绍，并通过一些示例来演示如何在 Java 开发过程正确的使用 Bean Validation。
<!-- more -->
##**关于 Bean Validation**

在任何时候，当你要处理一个应用程序的业务逻辑，数据校验是你必须要考虑和面对的事情。应用程序必须通过某种手段来确保输入进来的数据从语义上来讲是正确的。在通常的情况下，应用程序是分层的，不同的层由不同的开发人员来完成。很多时候同样的数据验证逻辑会出现在不同的层，这样就会导致代码冗余和一些管理的问题，比如说语义的一致性等。为了避免这样的情况发生，最好是将验证逻辑与相应的域模型进行绑定。

Bean Validation 为 JavaBean 验证定义了相应的元数据模型和 API。缺省的元数据是 `Java Annotations`，通过使用 XML 可以对原有的元数据信息进行覆盖和扩展。在应用程序中，通过使用 Bean Validation 或是你自己定义的 `constraint`，例如 `@NotNull`, `@Max`, `@ZipCode`， 就可以确保数据模型（JavaBean）的正确性。constraint 可以附加到字段，getter 方法，类或者接口上面。对于一些特定的需求，用户可以很容易的开发定制化的 constraint。Bean Validation 是一个运行时的数据验证框架，在验证之后验证的错误信息会被马上返回。

`JSR 303 – Bean Validation` 规范 [http://jcp.org/en/jsr/detail?id=303](http://jcp.org/en/jsr/detail?id=303)

`Hibernate Validator` 是 Bean Validation 的参考实现 . Hibernate Validator 提供了 JSR 303 规范中所有内置`constraint` 的实现，除此之外还有一些附加的 constraint。如果想了解更多有关 Hibernate Validator 的信息，请查看 [http://hibernate.org/validator/](http://hibernate.org/validator/)

##**Bean Validation 中的 constraint**

表 1. Bean Validation 中内置的 constraint
<table><tr><td>Constraint</td><td>详细信息</td></tr><tr><td>@Null</td><td> 被注释的元素必须为 null</td></tr><tr><td>@NotNull</td><td> 被注释的元素必须不为 null</td></tr><tr><td>@AssertTrue</td><td> 被注释的元素必须为 true</td></tr><tr><td>@AssertFalse</td><td> 被注释的元素必须为 false</td></tr><tr><td>@Min(value)</td><td> 被注释的元素必须是一个数字，其值必须大于等于指定的最小值</td></tr><tr><td>@Max(value)</td><td> 被注释的元素必须是一个数字，其值必须小于等于指定的最大值</td></tr><tr><td>@DecimalMin(value)</td><td> 被注释的元素必须是一个数字，其值必须大于等于指定的最小值</td></tr><tr><td>@DecimalMax(value)</td><td> 被注释的元素必须是一个数字，其值必须小于等于指定的最大值</td></tr><tr><td>@Size(max, min)</td><td> 被注释的元素的大小必须在指定的范围内</td></tr><tr><td>@Digits (integer, fraction)</td><td> 被注释的元素必须是一个数字，其值必须在可接受的范围内</td></tr><tr><td>@Past</td><td> 被注释的元素必须是一个过去的日期</td></tr><tr><td>@Future</td><td> 被注释的元素必须是一个将来的日期</td></tr><tr><td>@Pattern(value)</td><td> 被注释的元素必须符合指定的正则表达式</td></tr></table>

表 2. Hibernate Validator 附加的 constraint
<table><tr><td>Constraint</td><td>详细信息</td></tr><tr><td>@Email</td> <td> 被注释的元素必须是电子邮箱地址</td> </tr> <tr> <td>@Length</td> <td> 被注释的字符串的大小必须在指定的范围内</td> </tr> <tr> <td>@NotEmpty</td> <td> 被注释的字符串的必须非空</td> </tr> <tr> <td>@Range</td> <td> 被注释的元素必须在合适的范围内</td> </tr> </table>

一个 constraint 通常由 annotation 和相应的 constraint validator 组成，它们是一对多的关系。也就是说可以有多个 constraint validator 对应一个 annotation。在运行时，Bean Validation 框架本身会根据被注释元素的类型来选择合适的 constraint validator 对数据进行验证。

有些时候，在用户的应用中需要一些更复杂的 constraint。Bean Validation 提供扩展 constraint 的机制。可以通过两种方法去实现，一种是组合现有的 constraint 来生成一个更复杂的 constraint，另外一种是开发一个全新的 constraint。

Bean Validation2个规范。

- Bean Validation 1.0（[JSR-303](http://jcp.org/en/jsr/detail?id=303)）

定义了基于注解方式的JavaBean验证元数据模型和API，也可以通过XML进行元数据定义，但注解将覆盖XML的元数据定义。
JSR-303主要是对JavaBean进行验证，如方法级别（方法参数/返回值）、依赖注入等的验证是没有指定的。因此又有了JSR-349规范的产生。

- Bean Validation 1.1（[JSR-349](http://jcp.org/en/jsr/detail?id=349)）
Bean Validation 标准化了Java平台的约束定义、描述、和验证。
Spring3.1目前已经完全支持依赖注入验证和方法级别验证的支持


Bean Validation在开发中的位置

1.**表现层验证**：SpringMVC提供对JSR-303的表现层验证；
2.**业务逻辑层验证**：Spring3.1提供对业务逻辑层的方法验证（当然方法验证可以出现在其他层，但笔者觉得方法验证应该验证业务逻辑）；
3.**DAO层验证**：Hibernate提供DAO层的模型数据的验证（可参考hibernate validator参考文档的7.3. ORM集成）。
4.**数据库端的验证**：通过数据库约束来进行；
5.**客户端验证支持**：JSR-303也提供编程式验证支持。


##**Hibernate Validation**

具体使用参考文档

各版本文档下载：[http://docs.jboss.org/hibernate/validator/](http://docs.jboss.org/hibernate/validator/)


##**参考文献**
1.[JSR 303 - Bean Validation 介绍及最佳实践](http://www.ibm.com/developerworks/cn/java/j-lo-jsr303/)
2.[Spring3.1 对Bean Validation规范的新支持(方法级别验证)](http://jinnianshilongnian.iteye.com/blog/1495594)