title: "安装JDK 6和手动配置Tomcat 7"
id: 850
date: 2013-06-12 10:52:36
tags: 
- JAVA
- JDK
- JDK6
- tomcat7
categories: 
- JAVA
---

## JDK篇

### 1、JDK 6下载

下载地址：[http://www.oracle.com/technetwork/java/javase/downloads/jdk6downloads-1902814.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk6downloads-1902814.html)

### 2、JDK 安装

请安步骤安装，不再详细描述。
<!-- more -->
### 3、JDK配置

添加环境变量 ， 右键 我的电脑-&gt;属性-&gt;高级-&gt;环境变量

新建系统变量，变量名为：`JAVA_HOME`  变量值为：JDK的安装根目录，我这里是 `D:\Program Files\Java\jdk1.6.0_45`

编辑原有的系统变量 `Path` ,在变量值的最后面加上英文的分号 ，然后加上 `%JAVA_HOME%\bin;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\jre\bin`;

### 4、验证配置是否成功

执行CMD命令   输入 `javac`  如果看到很多帮助命令，则表示环境变量配置成功了。XP系统可能需要重启才生效。

## Tomcat 7篇

### 1、下载 `Tomcat 7.0`

下载地址：[http://tomcat.apache.org/download-70.cgi](http://tomcat.apache.org/download-70.cgi)

我下载的是压缩便携版，如果你下载的是安装版直接安装就可以。便携版配置步骤如下。

### 2、解压

解压Tomcat7.0 免安装Win-32Bit版 到D:\  目录结构为：D:\apache-tomcat-7.0.41

### 3、添加环境变量

右键 我的电脑-&gt;属性-&gt;高级-&gt;环境变量
新建系统变量，变量名：`TOMCAT_HOME` 变量值：`D:\apache-tomcat-7.0.41`

### 4、编辑原有的系统变量 Path

在变量值的最后面加上`%TOMCAT_HOME%\lib;%TOMCAT_HOME%\lib\servlet-api.jar;%TOMCAT_HOME%\lib\jsp-api.jar`

### 5、配置Tomcat 管理员

进入D:\apache-tomcat-7.0.41\conf 目录，编辑`tomcat-users.xml`

添加如下代码：
{%codeblock lang:xml%}
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="admin" password="admin" roles="manager-gui,admin-gui"/>
{%endcodeblock%}

Tomcat管理员的账号和密码为 admin

如果角色不为：`manager-gui`,`admin-gui`就会出现403错误~

### 6、启动tomcat

进入`D:\apache-tomcat-7.0.41\bin` 找到 `startup.bat`  双击启动Tomcat，出现命令窗口，不要关闭。

### 7、验证是否成功

浏览器地址输入：[http://127.0.0.1:8080](http://127.0.0.1:8080/) 如果看到tomcat 欢迎界面则表示配置成功了。