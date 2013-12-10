title: Win7 64 bit 安装 Oracle 11gR2
date: 2013-12-10 16:00:34
categories: 程序人生
tags: [Oracle 11g R2,win7 64Bit,Oracle 安装]
---

最近准备学习Oracle，因为机器是win7 64 Bit，所以就去官网下载64bit的oracle。

我下载的版本是：Oracle 11g R2 64 Bit

官网地址：[http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html)

下载2个压缩包：`win64_11gR2_database_1of2` 和 `win64_11gR2_database_2of2`

可以从我的百度网盘下载，速度很快。

链接: [http://pan.baidu.com/s/1pbC7L](http://pan.baidu.com/s/1pbC7L) 密码: `itfs`<!-- more -->

1. 解压`win64_11gR2_database_1of2` 和 `win64_11gR2_database_2of2`到同一个文件夹，即合并。

2. 进入解压合并后的文件夹，右键击`setup.exe`选择以管理员运行程序，启动安装。
![](/img/2013/12/win764-install-oracle1.jpg)

3.在出现的“配置安全更新”窗口中，取消“我希望通过My Oracle Support接受安全更新”，单击“下一步”：
![](/img/2013/12/win764-install-oracle2.jpg)

不需要提供邮件，不然会出现错误信息`[INS-08109] 验证状态 'getOCMDetails' 的输入时出现意外错误。`。一般没必要。

4.在“安装选项”窗口中，选择“创建和配置数据库”，单击“下一步”： 
![](/img/2013/12/win764-install-oracle3.jpg)

5.在“系统类”窗口中，选择“桌面类”，单击“下一步”：
![](/img/2013/12/win764-install-oracle4.jpg)

6.在“典型安装”窗口中，选择Oracle的基目录，选择“企业版”和“默认值”并输入统一的密码为：`Oracle11g`，单击“下一步”： 
![](/img/2013/12/win764-install-oracle5.jpg)

7.在“先决条件检查”窗口中，单击“下一步”： 
![](/img/2013/12/win764-install-oracle6.jpg)

8.在“概要”窗口中，单击“完成”，即可进行安装： 
![](/img/2013/12/win764-install-oracle7.jpg)

9.出现的安装过程如下：
![](/img/2013/12/win764-install-oracle8.jpg)

10.数据库创建完成后，会出现如下“Database Configuration Assistant”界面： 
![](/img/2013/12/win764-install-oracle9.jpg)

11.选择“口令管理”，查看并修改以下用户：
- 普通管理员：SYSTEM（密码：manager）
- 超级管理员：SYS（密码：change_on_install）
修改完成后，单击“确定”。

12.在“完成”窗口中，单击“关闭”即可。 
![](/img/2013/12/win764-install-oracle10.jpg)

13.通过下面这个地址即可进入oracle登陆界面：
Enterprise Manager Database Control URL - (orcl) :
https://localhost:1158/em
![](/img/2013/12/win764-install-oracle11.jpg)


##**安装PL/SQL Developer**

1.下载 instantclient-basic-win32-11.2.0.1.0.zip 

2.解压到D:\app\目录

3.拷贝数据库安装根目录下的一个目录`D:\Oracle\app\hanfeng\product\11.2.0\dbhome_1\NETWORK`到Oracle客户端目录下`D:\Oracle\app\hanfeng\product\instantclient_11_2`

4.安装 PL/SQL Developer，在perference->Connection里面设置`OCI Library`和`Oracle_Home`，例如本机设置为：
- Oracle Home ：`D:\Oracle\app\hanfeng\product\instantclient_11_2`
- OCI Library ：`D:\Oracle\app\hanfeng\product\instantclient_11_2\oci.dll`

5.添加环境变量

系统变量中添加2个：
第一个是指向TNS文件所在目录的，这个目录是你安装的64位版本Oracle的TNS文件所在目录。TNS文件就是保存了连接信息的文件。

	TNS_ADMIN  值： E:\app\hanfeng\product\11.2.0\dbhome_1\NETWORK\ADMIN

第二个是指定数据库使用的编码。如果不设置成以下值，那么连接上数据库后，你看到的所有中文的内容将会是乱码，都是一堆问号。

	NLS_LANG  值：SIMPLIFIED CHINESE_CHINA.ZHS16GBK

启动 PL/SQL Developer即可。












