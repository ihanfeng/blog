title: Maven3+Spring4+Spring MVC+mybatis3整合实例(1)
date: 2013-12-19 09:25:24
categories: Java进阶
tags: JAVA整合
---
基于Maven的Spring+SpringMVC+Mybatis的一个小项目的搭建，使用Maven3.1.1管理项目。

##**1.项目架构搭建**

下面开始整合进程，首先创建Maven项目
![](/img/2013/12/java-zhenghe-1.jpg)
<!-- more -->
进入下一步，填写具体信息
![](/img/2013/12/java-zhenghe-2.jpg)

maven 项目创建完毕，我们发现没有src/main/java 和src/test/java两个source folder,只有一个src/main/resources source folder. 要解决这个问题，只需要项目属性中改一下System library就可以看到了。如图示

![](/img/2013/12/java-zhenghe-3.jpg)

接着修改project Facets里面的java版本为1.7

修改web.xml的头文件为3.0
{%codeblock lang:xml%}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
{%endcodeblock%}
 
最后项目的架构如下图：
![](/img/2013/12/java-zhenghe-4.jpg)

##**2.引入必需的jar**

接下来，由于我们要使用Spring 和mybatis，所以我们需要引用相关的依赖进POM.xml文件里面。
为了获得spring所需的jar，我们只需要`spring-context`,`spring-webmvn`,`spring-orm`3个jar就可以获得所需依赖的包。

可以通过[官网](search.maven.org)或者[http://mvnrepository.com/](http://mvnrepository.com/)检索需要的jar。然后复制到pom.xml文件里头。

我的完整pom.xml如下
{%codeblock lang:xml%}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.hanfeng.mis</groupId>
  <artifactId>mis</artifactId>
  <packaging>war</packaging>
  <version>1.0.1</version>
  <name>Maven Webapp Archetype</name>
  <url>http://maven.apache.org</url>
  
  <!-- 项目属性 -->
  <properties>
  	<!-- jar 版本设置 -->
  	<spring.version>4.0.0.RELEASE</spring.version>
  	<mybatis.version>3.2.3</mybatis.version>
  	<mybatis-spring.version>1.2.1</mybatis-spring.version>
  	<mysql.version>5.1.27</mysql.version>
  	<druid.version>1.0.1</druid.version>
  	<junit.version>4.11</junit.version>
  	<fastjson.version>1.1.37</fastjson.version>
  	<sitemesh.version>2.4.2</sitemesh.version>
  	<log4j.version>1.2.17</log4j.version>
  	<slf4j.version>1.7.5</slf4j.version>
  	<commons-lang3.version>3.1</commons-lang3.version>
	<commons-io.version>2.4</commons-io.version>
	<commons-codec.version>1.8</commons-codec.version>
	<commons-fileupload.version>1.3</commons-fileupload.version>
	<commons-beanutils.version>1.8.3</commons-beanutils.version>
	<jdk.version>1.7</jdk.version>
  </properties>
  
  <dependencies>
  	<!-- Spring -->
    <dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>${spring.version}</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>${spring.version}</version>
	</dependency>
    <dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-orm</artifactId>
		<version>${spring.version}</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-test</artifactId>
		<version>${spring.version}</version>
	</dependency>
            
	<!-- mybatis -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>${mybatis.version}</version>
	</dependency>
    <dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis-spring</artifactId>
		<version>${mybatis-spring.version}</version>
	</dependency>
    <!-- mysql -->
    <dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>${mysql.version}</version>
	</dependency>
    <!-- druid -->
    <dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>druid</artifactId>
		<version>${druid.version}</version>
	</dependency>
    <!-- junit -->                               
    <dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>${junit.version}</version>
	</dependency>
	<!-- fastjson -->
	<dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>fastjson</artifactId>
		<version>${fastjson.version}</version>
	</dependency>
	<!-- sitemesh -->
	<dependency>
		<groupId>opensymphony</groupId>
		<artifactId>sitemesh</artifactId>
		<version>${sitemesh.version}</version>
	</dependency>           
    <!-- log4j -->
    <dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
		<version>${log4j.version}</version>
	</dependency>   
	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-log4j12</artifactId>
		<version>${slf4j.version}</version>
	</dependency>
	
	<!-- GENERAL UTILS begin -->
	<dependency>
		<groupId>org.apache.commons</groupId>
		<artifactId>commons-lang3</artifactId>
		<version>${commons-lang3.version}</version>
	</dependency>
	<dependency>
		<groupId>commons-io</groupId>
		<artifactId>commons-io</artifactId>
		<version>${commons-io.version}</version>
	</dependency>
	<dependency>
		<groupId>commons-codec</groupId>
		<artifactId>commons-codec</artifactId>
		<version>${commons-codec.version}</version>
	</dependency>
	<dependency>
	    <groupId>commons-fileupload</groupId>
	    <artifactId>commons-fileupload</artifactId>
	    <version>${commons-fileupload.version}</version>
	</dependency>
	<dependency>
		<groupId>commons-beanutils</groupId>
		<artifactId>commons-beanutils</artifactId>
		<version>${commons-beanutils.version}</version>
		<exclusions>
			<exclusion>
				<groupId>commons-logging</groupId>
				<artifactId>commons-logging</artifactId>
			</exclusion>
		</exclusions>
	</dependency>
    <dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>servlet-api</artifactId>
		<version>2.5</version>
		<scope>provided</scope>
	</dependency>  
  </dependencies>
  
  <!-- 设定除中央仓库(repo1.maven.org/maven2/)外的其他仓库,按设定顺序进行查找. -->
	<repositories>
		<!-- 如有Nexus私服, 取消注释并指向正确的服务器地址.
		<repository>
			<id>nexus-repos</id>
			<name>Team Nexus Repository</name>
			<url>http://localhost:8081/nexus/content/groups/public</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository> -->
		
		<repository>
			<id>central-repos</id>
			<name>Central Repository</name>
			<url>http://repo.maven.apache.org/maven2</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
		
		<repository>
			<id>central-repos2</id>
			<name>Central Repository 2</name>
			<url>http://repo1.maven.org/maven2/</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
		
		<repository>
			<id>springsource-repos</id>
			<name>SpringSource Repository</name>
			<url>http://repo.springsource.org/libs-milestone-local</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
	    </repository>	
	</repositories>	
  
  <build>
    <finalName>mis</finalName>
    <plugins>
		<!-- Compiler 插件, 设定JDK版本 -->
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>2.5.1</version>
			<configuration>
				<source>${jdk.version}</source>
				<target>${jdk.version}</target>
				<showWarnings>true</showWarnings>
			</configuration>
		</plugin>
	</plugins>
  </build>
</project>
{%endcodeblock%}

##**使用MyBatis生成工具**

主要用来生成dao,mapping,model，文件配置如下：
{%codeblock lang:xml%}
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration >
	<!-- 数据库驱动包位置 -->
	<classPathEntry location="D:\maven\repository\mysql\mysql-connector-java\5.1.27\mysql-connector-java-5.1.27.jar" />
	<context id="context1" >
	 	<!-- 是否去除自动生成的注释 true：是 ： false:否 -->  
		<commentGenerator>
	      <property name="suppressAllComments" value="true" />
	      <property name="suppressDate" value="false" />
  		</commentGenerator>
  		
		<!-- 数据库链接URL、user、pwd -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost:3306/mis" userId="root" password="123456" />
		
		<!-- 指定生成的类型为java类型，避免数据库中number等类型字段 -->  
        <javaTypeResolver>  
            <property name="forceBigDecimals" value="false"/>  
        </javaTypeResolver> 
		
		<!-- 生成模型的包名和位置 -->
		<javaModelGenerator targetPackage="com.hanfeng.mis.modules.sys.entity" targetProject="mis" />
		<!-- 生成的映射文件包名和位置 -->
		<sqlMapGenerator targetPackage="com.hanfeng.mis.modules.sys.mappings" targetProject="mis" />
		<!-- 生成DAO的包名和位置 -->
		 <javaClientGenerator targetPackage="com.hanfeng.mis.modules.sys.dao" targetProject="mis" type="XMLMAPPER" />
		<!-- 要生成的表 -->
		 <table tableName="sys_user" domainObjectName="User" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" />
	</context>
</generatorConfiguration>
{%endcodeblock%}


##**Java代码编写**

首先编写service代码

编写service接口

{%codeblock lang:java%}
package com.hanfeng.mis.modules.sys.service;

import com.hanfeng.mis.modules.sys.entity.User;

public interface UserService {
	/**
	 * 根据id获取用户
	 */
	public User getUserById(Long id);
}
{%endcodeblock%}

编写service实现代码：

{%codeblock lang:java%}
package com.hanfeng.mis.modules.sys.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.hanfeng.mis.modules.sys.dao.UserMapper;
import com.hanfeng.mis.modules.sys.entity.User;

@Service
public class UserServiceImpl implements UserService {

	private UserMapper userMapper;

	public UserMapper getUserMapper() {
		return userMapper;
	}

	@Autowired
	public void setUserMapper(UserMapper userMapper) {
		this.userMapper = userMapper;
	}

	@Override
	public User getUserById(Long id) {
		return userMapper.selectByPrimaryKey(id);
	}

}
{%endcodeblock%}

##**使用spring集成测试**

即导入spring-test jar包。

测试代码：
{%codeblock lang:java%}
package com.hanfeng.mis.test.sys.user;

import org.apache.log4j.Logger;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import com.alibaba.fastjson.JSON;
import com.hanfeng.mis.modules.sys.entity.User;
import com.hanfeng.mis.modules.sys.service.UserService;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:spring-context.xml","classpath:spring-context-mybatis.xml","classpath:spring-mvc.xml"})
@ActiveProfiles("production") 
public class TestUser {
	private static final Logger logger = Logger.getLogger(TestUser.class);

	private UserService userService;

	public UserService getUserService() {
		return userService;
	}

	@Autowired
	public void setUserService(UserService userService) {
		this.userService = userService;
	}

	@Test
	public void test() {
		User u = userService.getUserById(1L);
		logger.debug("根据ID获取用户："+JSON.toJSONString(u));
	}
}
{%endcodeblock%}

测试结果：

	2013-12-19 13:42:08,545 [main] DEBUG [com.hanfeng.mis.modules.sys.dao.UserMapper.selectByPrimaryKey] - ooo Using Connection [com.alibaba.druid.proxy.jdbc.ConnectionProxyImpl@333770f]
	2013-12-19 13:42:08,561 [main] DEBUG [com.hanfeng.mis.modules.sys.dao.UserMapper.selectByPrimaryKey] - ==>  Preparing: select id, company_id, office_id, login_name, password, no, name, email, phone, mobile, user_type, login_ip, login_date, create_by, create_date, update_by, update_date, remarks, del_flag from sys_user where id = ? 
	2013-12-19 13:42:08,674 [main] DEBUG [com.hanfeng.mis.modules.sys.dao.UserMapper.selectByPrimaryKey] - ==> Parameters: 1(Long)
	2013-12-19 13:42:08,746 [main] DEBUG [com.hanfeng.mis.modules.sys.dao.UserMapper.selectByPrimaryKey] - <==      Total: 1
	2013-12-19 13:42:09,005 [main] DEBUG [com.hanfeng.mis.test.sys.user.TestUser] - 根据ID获取用户：{"companyId":1,"createBy":1,"createDate":1369612800000,"delFlag":"0","email":"zhdevelop@gmail.com","id":1,"loginDate":1387247510000,"loginIp":"127.0.0.1","loginName":"super","mobile":"8675","name":"super","no":"0001","officeId":1,"password":"02a3f0772fcca9f415adc990734b45c6f059c7d33ee28362c4852032","phone":"8675","remarks":"最高管理员","updateBy":1,"updateDate":1369612800000}

##**编写Controll代码**
{%codeblock lang:java%}
package com.hanfeng.mis.modules.sys.web;

import javax.annotation.Resource;
import javax.servlet.http.HttpServletRequest;

import org.apache.log4j.Logger;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;

import com.alibaba.fastjson.JSON;
import com.hanfeng.mis.modules.sys.entity.User;
import com.hanfeng.mis.modules.sys.service.UserService;

@Controller
@RequestMapping("/user")
public class UserController {
	
	private static final Logger logger = Logger.getLogger(UserController.class);
	
	private UserService userService;

	@Resource
	public void setUserService(UserService userService) {
		this.userService = userService;
	}
	
	@RequestMapping("/{id}")
	public String showUser(@PathVariable int id,HttpServletRequest request){
		User user = userService.getUserById(1L);
		request.setAttribute("user", user);
		logger.debug("根据ID获取用户："+JSON.toJSONString(user));
		return "modules/sys/showUser";	
	}
}

package com.hanfeng.mis.modules.sys.web;

import javax.annotation.Resource;
import javax.servlet.http.HttpServletRequest;

import org.apache.log4j.Logger;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;

import com.alibaba.fastjson.JSON;
import com.hanfeng.mis.modules.sys.entity.User;
import com.hanfeng.mis.modules.sys.service.UserService;

@Controller
@RequestMapping("/user")
public class UserController {
	
	private static final Logger logger = Logger.getLogger(UserController.class);
	
	private UserService userService;

	@Resource
	public void setUserService(UserService userService) {
		this.userService = userService;
	}
	
	@RequestMapping("/{id}")
	public String showUser(@PathVariable int id,HttpServletRequest request){
		User user = userService.getUserById(1L);
		request.setAttribute("user", user);
		logger.debug("根据ID获取用户："+JSON.toJSONString(user));
		return "modules/sys/showUser";	
	}
}
{%endcodeblock%}

具体看源代码：

链接: [http://pan.baidu.com/s/1pbUX1](http://pan.baidu.com/s/1pbUX1) 密码: 3jvj



