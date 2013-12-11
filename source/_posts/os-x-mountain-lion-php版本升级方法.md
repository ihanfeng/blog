title: "OS X Mountain Lion  PHP版本升级方法"
id: 915
date: 2013-09-13 21:17:01
tags: 
- mac
- php
categories: 
- PHP5
---

推荐使用[php-osx by Liip](http://php-osx.liip.ch/)

### PHP 5.3

<pre>curl -s http://php-osx.liip.ch/install.sh | bash -s 5.3</pre>

### PHP 5.4 (Old stable)

<pre>curl -s http://php-osx.liip.ch/install.sh | bash -s 5.4</pre>

### PHP 5.5 (Current stable)

<pre>curl -s http://php-osx.liip.ch/install.sh | bash -s 5.5
在终端输入上面的更新地址，按要求输入密码即可完成升级安装。因为天朝网络的特殊性，该怎么办你们懂的，安装完毕要更改环境变量才可以显示新版本的。

export PATH=/usr/local/php5/bin:$PATH</pre>