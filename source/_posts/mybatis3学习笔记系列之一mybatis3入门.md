title: "Mybatis3学习笔记系列之一：Mybatis3入门"
id: 873
date: 2013-06-19 09:48:26
tags: 
- mybatis
categories: 
- mybatis
---

## 什么是 MyBatis?

`MyBatis` 是支持普通 SQL 查询,存储过程和高级映射的优秀持久层框架。MyBatis 消除 了几乎所有的 JDBC 代码和参数的手工设置以及结果集的检索。MyBatis 使用简单的 XML 或注解用于配置和原始映射,将接口和 Java 的 POJOs(Plan Old Java Objects,普通的 Java 对象)映射成数据库中的记录。

MyBatis作为持久层框架，其主要思想是将程序中的大量sql语句剥离出来，配置在配置文件中，实现sql的灵活配置。这样做的好处是将sql与程序代码分离，可以在不修改程序代码的情况下，直接在配置文件中修改sql。
<!-- more -->
## 从 `iBatis` 到 `MyBatis`，你准备好了吗？

对于从事 `Java EE` 的开发人员来说，iBatis 是一个再熟悉不过的持久层框架了，在 `Hibernate`、`JPA` 这样的一站式对象 / 关系映射（`O/R Mapping`）解决方案盛行之前，iBaits 基本是持久层框架的不二选择。即使在持久层框架层出不穷的今天，iBatis 凭借着易学易用、轻巧灵活等特点，也仍然拥有一席之地。尤其对于擅长 SQL 的开发人员来说，iBatis 对 SQL 和存储过程的直接支持能够让他们在获得 iBatis 封装优势的同时而不丧失 SQL 调优的手段，这是 Hibernate/JPA 所无法比拟的。具体而言，使用 iBatis 框架的主要优势主要体现在如下几个方面：
首先，iBatis 封装了绝大多数的 `JDBC` 样板代码，使得开发者只需关注 SQL 本身，而不需要花费精力去处理例如注册驱动，创建 `Connection`，以及确保关闭 `Connection` 这样繁杂的代码。
其次，iBatis 可以算是在所有主流的持久层框架中学习成本最低，最容易上手和掌握的框架。虽说其他持久层框架也号称门槛低，容易上手，但是等到你真正使用时会发现，要想掌握并用好它是一件非常困难的事。在工作中我需要经常参与面试，我曾听到过很多位应聘者描述，他们所在的项目在技术选型时选择 Hibernate，后来发现难以驾驭，不得不将代码用 JDBC 或者 iBatis 改写。
iBatis 自从在 `Apache` 软件基金会网站上发布至今，和他的明星兄弟们（`Http Server`，`Tomcat`，`Struts`，`Maven`，`Ant` 等等）一起接受者万千 Java 开发者的敬仰。然而在今年六月中旬，几乎是发布 3.0 版本的同时，iBatis 主页上的一则 “Apache iBATIS has been retired” 的声明在社区引起了一阵不小的波澜。在 Apache 寄居六年之后，iBatis 将代码托管到 Google Code。在声明中给出的主要理由是，和 Apache 相比，`Google Code` 更有利于开发者的协同工作，也更能适应快速发布。于此同时，iBatis 更名为 MyBatis。
由一个 MyBatis 示例开始

如果读者接触过一些常用的 Java EE 框架，应该都知道这些框架需要提供一个全局配置文件，用于指定程序正常运行所需的设置和参数信息。而针对常用的持久层框架而言（Hibernate、JPA、iBatis 等），则通常需要配置两类文件：一类用于指定数据源、事务属性以及其他一些参数配置信息（通常是一个独立的文件，可以称之为全局配置文件）；另一类则用于指定数据库表和程序之间的映射信息（可能不止一个文件，我们称之为映射文件）。MyBatis 也不例外，虽然其中的一部分可以通过注解的形式进行，但是这两部分内容本身仍是必不可少的。

根据 iBatis 的习惯，我们通常把全局配置文件命名为 `sqlMapConfig.xml`，文件名本身并没有要求，在 MyBatis 中，也经常会将该文件命名为 `Configuration.xml `（读完全文后读者也许会发现，在 iBatis 中经常出现的 “sqlMap” 在 MyBatis 中被逐渐淡化了，除了此处，还比如 iBatis 配置文件的根元素为 &lt;sqlMapConfig&gt;，指定映射文件的元素为 &lt;sqlMap&gt;，以及 SqlMapClient 等等，这个变化正说明，iBatis 仅是以 SQL 映射为核心的框架，而在 MyBatis 中多以 `Mapper`、`Session`、`Configuration` 等其他常用 `ORM` 框架中的名字代替，体现的无非是两个方面：首先是为了减少开发者在切换框架所带来的学习成本；其次，MyBatis 充分吸收了其他 ORM 框架好的实践，MyBatis 现在已不仅仅是一个 SQL 映射框架了）。在全局配置文件中可以配置的信息主要包括如下几个方面：

*   `properties` --- 用于提供一系列的键值对组成的属性信息，该属性信息可以用于整个配置文件中。
*   `settings` --- 用于设置 MyBatis 的运行时方式，比如是否启用延迟加载等。
*   `typeAliases` --- 为 Java 类型指定别名，可以在 XML 文件中用别名取代 Java 类的全限定名。
*   `typeHandlers` --- 在 MyBatis 通过 PreparedStatement 为占位符设置值，或者从 ResultSet 取出值时，特定类型的类型处理器会被执行。
*   `objectFactory` --- MyBatis 通过 ObjectFactory 来创建结果对象。可以通过继承 DefaultObjectFactory 来实现自己的 ObjectFactory 类。
*   `plugins` --- 用于配置一系列拦截器，用于拦截映射 SQL 语句的执行。可以通过实现 Interceptor 接口来实现自己的拦截器。
*   `environments` --- 用于配置数据源信息，包括连接池、事务属性等。
*   `mappers` --- 程序中所有用到的 SQL 映射文件都在这里列出，这些映射 SQL 都被 MyBatis 管理。

下面我们根据具体的实例，来了解下mybatis。实例环境：`eclipse 4.2` `mysql 5.5` `mybatis3.2.2`

首先创建web项目，命名为mybatisDemo01，接着复制相关jar包到lib目录下。`mybatis-3.2.2.jar`和`mysql-connector-java-5.1.18.jar`。
1、创建文件夹`main.resources.config`，用来保存配置文件。创建名为`configuration.xml`的配置文件，其内容如下：

{%codeblock lang:xml%}
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;
<configuration>
<!—类型别名定义。今后可只用Student来代替它冗长的 全限定名—>
<typeAliases>
<typeAlias alias="Student" type="com.hanfeng.demo.pojo.Student"/>
</typeAliases>
<!— 配置数据源相关的信息 —>
<environments default="development">
<environment id="development">
<transactionManager type="JDBC"/>
<!—使用连接池的数据源配置—>
<dataSource type="POOLED">
<property name="driver" value="com.mysql.jdbc.Driver"/>
<property name="url" value="jdbc:mysql://localhost:3306/db_test"/>
<property name="username" value="root"/>
<property name="password" value="123456"/>
</dataSource>
</environment>
</environments>
<!— 列出映射文件 —>
<!—指定要用到的mapper文件。以下的resource属性告诉
MyBatis要在类路径下的resources目录下找StudentMapper.xml文件。
我们将把mapper文件存放在src目录下的resources目录中—>
<mappers>
<mapper resource="main/resources/sqlmap/StudentMapper.xml"/>
</mappers>
</configuration>
{%endcodeblock%}

一定要注意上面的映射文件 DTD 约束哦，别忘记添加了。这个配置文件比较简单，主要是数据源和映射文件的配置。

2、创建学生类。包名：`com.hanfeng.demo.pojo`

{%codeblock lang:java%}
package com.hanfeng.demo.pojo;
 public class Student {
 private int id;
 private String name; //姓名
 private String gender; //性别
 private String major; //专业
 private String grade; //年级
 public int getId() {
 return id;
 }
 public void setId(int id) {
 this.id = id;
 }
 public String getName() {
 return name;
 }
 public void setName(String name) {
 this.name = name;
 }
 public String getGender() {
 return gender;
 }
 public void setGender(String gender) {
 this.gender = gender;
 }
 public String getMajor() {
 return major;
 }
 public void setMajor(String major) {
 this.major = major;
 }
 public String getGrade() {
 return grade;
 }
 public void setGrade(String grade) {
 this.grade = grade;
 }
 }
{%endcodeblock%}

3、创建接口，在接口中声明访问数据库的方法。包名：`com.hanfeng.demo.dao`

{%codeblock lang:java%}
 package com.hanfeng.demo.dao;
 import com.hanfeng.demo.pojo.Student;
 /**
 * 接口 声明访问数据库的方法
 * @author hanfeng
 *
 */
 public interface StudentMapper {
 //根据学生ID查询学生实体
 public Student getById(int id);
 }
{%endcodeblock%}

4、编写`mapper`文件，`StudentMapper.xml`。在此文件里，我们写好查询的SQL语句，并配置好映射关系。

MyBatis将根据此文件帮我们实现StudentMapper接口。内容如下：

{%codeblock lang:xml%}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;
<!—namespace该是StudentMapper的完整限定名—>
<mapper namespace="com.hanfeng.demo.dao.StudentMapper">
<!—定义java bean的属性与数据库表的列之间的映射。type="Student"用到了configuration.xml中定义的别名—>
<resultMap id="studentResultMap" type="Student">
<!—id映射—>
<id property="id" column="id"/>
<!—普通属性映射—>
<result property="name" column="name"/>
<result property="gender" column="gender"/>
<result property="major" column="major"/>
<result property="grade" column="grade"/>
</resultMap>
<!—与StudentMapper接口中的getById方法对应，包括
方法名和参数类型。SQL语句中以“#{}”的形式引用参数—>
<select id="getById" parameterType="int" resultMap="studentResultMap">
SELECT *
FROM student WHERE id = #{id}
</select>
</mapper>
{%endcodeblock%}
5、测试功能。

创建一个专门用来保存测试代码的源文件夹test。包名：`com.hanfeng.demo.pojo.test`

{%codeblock lang:java%}
package com.hanfeng.demo.pojo.test;
 import java.io.Reader;
 import org.apache.ibatis.io.Resources;
 import org.apache.ibatis.session.SqlSession;
 import org.apache.ibatis.session.SqlSessionFactory;
 import org.apache.ibatis.session.SqlSessionFactoryBuilder;
 import com.hanfeng.demo.dao.StudentMapper;
 import com.hanfeng.demo.pojo.Student;
 public class StudentTest {
 public static void main(String[] args) {
 //与configuration.xml中的mapper配置类似，告诉MyBatis应读取的核心配置文件
 String resource = &quot;main/resources/config/configuration.xml&quot;;
 Reader reader = null;
 try {
 reader = Resources.getResourceAsReader(resource);
 } catch (Exception e) {
 e.printStackTrace();
 }
 //创建SqlSessionFactory实例。没有指定要用到的
 //environment，则使用默认的environment
 SqlSessionFactory sqlSessionFactory;
 sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
 SqlSession sqlSession = sqlSessionFactory.openSession();
 try {
 StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
 Student student = studentMapper.getById(2);
 if (student != null) {
 System.out.println(&quot;姓名：&quot;+student.getName()+&quot;\n专业：&quot;+student.getMajor());
 }else{
 System.out.println(&quot;没有找到相关记录！&quot;);
 }
 } catch (Exception e) {
 e.printStackTrace();
 }finally{
 sqlSession.close();
 }
 }
 }
 {%endcodeblock%}

然后运行测试。我们会发现程序正常输出内容，说明测试成功，代码没呀问题。

[![wpid-210073e420d43daf2ad7a97814144c74_6878224.png](http://173.234.48.113/wp-content/uploads/2013/06/wpid-210073e420d43daf2ad7a97814144c74_6878224.png)](http://173.234.48.113/wp-content/uploads/2013/06/wpid-210073e420d43daf2ad7a97814144c74_6878224.png)

例子的架构目录：

[![7020185](http://173.234.48.113/wp-content/uploads/2013/06/7020185-210x300.png)](http://173.234.48.113/wp-content/uploads/2013/06/7020185.png)

源代码：[http://pan.baidu.com/share/link?shareid=989191459&amp;uk=4161616584](http://pan.baidu.com/share/link?shareid=989191459&amp;uk=4161616584)