title: 重温spring3-JDBC支持
date: 2013-12-10 09:42:32
categories: Spring3
tags: [spring3,jdbc]
---
##**JDBC回顾**

传统应用程序开发中，需要编写如此代码：

{%codeblock lang:java%}
@Test  
public void test() throws Exception {  
    Connection conn = null;  
    PreparedStatement pstmt = null;  
    try {  
      conn = getConnection();              //1.获取JDBC连接  
                                       //2.声明SQL  
      String sql = "select * from INFORMATION_SCHEMA.SYSTEM_TABLES";  
      pstmt = conn.prepareStatement(sql);    //3.预编译SQL  
      ResultSet rs = pstmt.executeQuery();   //4.执行SQL  
      process(rs);                       //5.处理结果集  
      closeResultSet(rs);                 //5.释放结果集  
      closeStatement(pstmt);              //6.释放Statement  
      conn.commit();                    //8.提交事务  
    } catch (Exception e) {  
      //9.处理异常并回滚事务  
      conn.rollback();  
      throw e;  
    } finally {  
      //10.释放JDBC连接，防止JDBC连接不关闭造成的内存泄漏  
      closeConnection(conn);  
    }  
}  
{%endcodeblock%}
<!-- more -->
 以上代码片段具有冗长、重复、容易忘记某一步骤从而导致出错、显示控制事务、显示处理受检查异常等等。

有朋友可能重构出自己的一套JDBC模板，从而能简化日常开发，但自己开发的JDBC模板不够通用，而且对于每一套JDBC模板实现都差不多，从而导致开发人员必须掌握每一套模板。

Spring JDBC提供了一套JDBC抽象框架，用于简化JDBC开发，而且如果各个公司都使用该抽象框架，开发人员首先减少了学习成本，直接上手开发，如图7-1所示。
![](/img/2013/12/spring-jdbc-duibi.jpg)

##**Spring对JDBC的支持**

Spring通过抽象JDBC访问并提供一致的API来简化JDBC编程的工作量。我们只需要声明SQL、调用合适的Spring JDBC框架API、处理结果集即可。事务由Spring管理，并将JDBC受查异常转换为Spring一致的非受查异常，从而简化开发。

Spring主要提供`JDBC模板方式`、`关系数据库对象化方式`和`SimpleJdbc方式`三种方式来简化JDBC编程，这三种方式就是Spring JDBC的工作模式：

- **JDBC模板方式**：Spring JDBC框架提供以下几种模板类来简化JDBC编程，实现GoF模板设计模式，将可变部分和非可变部分分离，可变部分采用回调接口方式由用户来实现：如`JdbcTemplate`、`NamedParameterJdbcTemplate`、`SimpleJdbcTemplate`。
- **关系数据库操作对象化方式**：Spring JDBC框架提供了将关系数据库操作对象化的表示形式，从而使用户可以采用面向对象编程来完成对数据库的访问；如`MappingSqlQuery`、`SqlUpdate`、`SqlCall`、`SqlFunction`、`StoredProcedure`等类。这些类的实现一旦建立即可重用并且是线程安全的。
- **SimpleJdbc方式**：Spring JDBC框架还提供了SimpleJdbc方式来简化JDBC编程，`SimpleJdbcInsert`、 `SimpleJdbcCall`用来简化数据库表插入、存储过程或函数访问。

Spring JDBC还提供了一些强大的工具类，如DataSourceUtils来在必要的时候手工获取数据库连接等。

##**Spring的JDBC架构**

Spring JDBC抽象框架由四部分组成：`datasource`、`support`、`core`、`object`。如图7-2所示。
 
 ![](/img/2013/12/spring3-jdbc-jiagou.jpg)

- **support包**：提供将JDBC异常转换为DAO非检查异常转换类、一些工具类如JdbcUtils等。
- **datasource包**：提供简化访问JDBC 数据源（javax.sql.DataSource实现）工具类，并提供了一些DataSource简单实现类从而能使从这些DataSource获取的连接能自动得到Spring管理事务支持。
- **core包**：提供JDBC模板类实现及可变部分的回调接口，还提供SimpleJdbcInsert等简单辅助类。
- **object包**：提供关系数据库的对象表示形式，如MappingSqlQuery、SqlUpdate、SqlCall、SqlFunction、StoredProcedure等类，该包是基于core包JDBC模板类实现。