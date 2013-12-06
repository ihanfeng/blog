title: "magento之Bluehost 如何同时运行PHP 5.2，PHP 5.3 和PHP 5.4"
id: 608
date: 2013-06-04 17:14:59
tags: 
categories: 
- magento
---

#### Bluehost 配置PHP环境

1.  登录Bluehost的 cPanel。
2.  选择 "Software/Services" 栏目下的 "PHP Config"。![Bluehost PHP config](https://tutorials.bluehost.com/help_media/services_php_config.jpg "Bluehost PHP version config")
3.  选择hosting默认的PHP版本，其他版本的需要单独在.htaccess里设置，我默认就选择第一个PHP 5.2。
<!-- more -->
*   <span>PHP 5.2</span>
*   <span>PHP 5.2 (Single php.ini)</span>
*   <span>PHP 5.2 (FastCGI)</span>
*   <span>PHP 5.3</span>
*   <span>PHP 5.3 (Single php.ini)</span>
*   <span>PHP 5.4</span>
*   <span>PHP 5.4 (Single php.ini)</span>
当上面的选择一个后，系统会默认备份public_html下的php.ini，另外会在.htaccess的头部添加选择的PHP版本。
> &lt;FilesMatch "\.php$"&gt;
> AddHandler application/x-httpd-php5 .php
> &lt;/FilesMatch&gt;
这样系统所有的目录和域名都运行的是PHP 5.2版本，可以通过phpinfo()来查看。

可以点击这里看看结果：[http://demo.drupal100.com/php52/phpinfo.php](http://demo.drupal100.com/php52/phpinfo.php)

目前Bluehost的版本是PHP 5.2.17，是PHP 5.2 中最稳定的一个版本。

&nbsp;

#### 分别设置PHP 5.3 和PHP 5.4

关键就在这里，如何设置一个目录或域名运行PHP 5.3？在.htaccess中加上如下代码。
> &lt;FilesMatch "\.php$"&gt;
> AddHandler application/x-httpd-php53 .php
> &lt;/FilesMatch&gt;
可以点击这里看看结果：[http://demo.drupal100.com/php53/phpinfo.php](http://demo.drupal100.com/php53/phpinfo.php)

一般Bluehost会更新到最新的PHP 5.3 版本，目前是PHP 5.3.20。

[![最新的PHP 5.3 版本，目前是PHP 5.3.20。](http://blog.lixiphp.com/wp-content/uploads/2013/03/image_thumb1.png "Bluehost会更新到最新的PHP 5.3 版本，目前是PHP 5.3.20。")](http://blog.lixiphp.com/wp-content/uploads/2013/03/image1.png)

如何设置一个目录或者域名运行PHP 5.4？在.htaccess中加上如下代码。
> &lt;FilesMatch "\.php$"&gt;
> AddHandler application/x-httpd-php54 .php
> &lt;/FilesMatch&gt;
可以点击这里看看结果：[http://demo.drupal100.com/php54/phpinfo.php](http://demo.drupal100.com/php54/phpinfo.php)

一般Bluehost会更新到最新的PHP 5.4 版本，目前是PHP 5.4.10。

[![最新的PHP 5.4 版本，目前是PHP 5.4.10。](http://blog.lixiphp.com/wp-content/uploads/2013/03/image_thumb2.png "Bluehost最新的PHP 5.4 版本，目前是PHP 5.4.10。")](http://blog.lixiphp.com/wp-content/uploads/2013/03/image2.png)

如果你有一个Bluehost空间，赶快试一试吧！