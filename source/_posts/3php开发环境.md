title: 3.PHP开发环境
date: 2013-12-11 12:50:39
categories: PHP5
tags: [PHP5,开发环境]
---
为了快速学习PHP，就不准备自己缓慢的去搭建一个完善的开发环境，直接使用集成环境哈。

目前网上PHP集成环境主要有AppServ、WAMP、XAMPP等。这些都差不多吧，我使用的是WAMP集成环境。

下载官网：[http://www.wampserver.com/en/](http://www.wampserver.com/en/)
<!-- more -->
安装步骤很简单，一键下去就可以啦。不懂得自行google。

启动WAMP，查看右键任务栏的图标是否为绿色，绿色表示启动成功。在浏览器输入`http://localhost/`,出现如下页面，表示安装成功。
![](/img/2013/12/php5-pic2.jpg)

通过phpinfo()函数测试PHP环境是否可以正常运行。如下图，表示环境正常，可以进行下一步开发了。
![](/img/2013/12/php5-pic1.jpg)

phpinfo()函数作用是输出有关PHP当前状态的大部分信息内容，包括关于PHP的编译和扩展信息、php版本、服务器信息和环境、php环境、php当前所安装的扩展模块、操作系统信息、路径、主要的和本地配置选项的值、HTTP头信息和PHP的许可。

启用PHPMyAdmin的HTTP身份验证。

修改config.inc.php文件。

	$cfg['Servers'][$i]['auth_type'] = 'cookie';

更改为

	$cfg['Servers'][$i]['auth_type'] = 'cookie';

这样登陆phpMyAdmin就需要输入账号和密码进行验证。

修改phpMyAdmin默认的空密码为：`123456`

	update user set password=password('123456') where User='root'

然后在修改config.inc.php文件

	$cfg['Servers'][$i]['AllowNoPassword'] = true;
更改为

	$cfg['Servers'][$i]['AllowNoPassword'] = false;

这样就完成了环境的配置过程。
