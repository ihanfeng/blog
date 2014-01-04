title: Discuz X3附件使用七牛云存储加速
date: 2013-12-30 12:51:59
categories: Discuz
tags: [discuz,qiniu,七牛,加速]
---
正在使用discuz搭建本地社区，因为空间准备在国外购买，所以想使用七牛云存储来保存论坛的附件，特别是图片，来加速。

DiscuzX使用云存储原理：
我们通过改造ftp类，当附件上传到本地时再通过ftp类将附件上传到云存储上。

discuz官方提供了使用七牛云存储来存储附件的扩展包。用户可以按照下面的步骤来安装使用。
<!--more-->
1.确认意见安装Discuz! X2.5 或X3.0 或X3.1。
2.防止意外，首先备份下discuz。
3.下载discuz版本对应的[DISCUZX2.5/X3扩展框架DXEXTEND](http://www.discuz.net/thread-3334048-1-1.html) 。解压缩并将其中的文件夹复制到discuz根目录下。
4.下载[DISCUZX2.5/3云存储通用接口](http://www.discuz.net/thread-3399569-1-1.html)
5.在`config/config_global.php`中新增以下内容:
{%codeblock lang:php%}
$_config['extend']['storage']['curstorage'] = 'qiniu';
$_config['extend']['storage']['qiniu']['accesskey'] = '填写你的AccessKey';
$_config['extend']['storage']['qiniu']['secretkey'] = '填写你的SecretKey';
$_config['extend']['storage']['qiniu']['attachurl'] = '填写你的七牛域名';
$_config['extend']['storage']['qiniu']['bucket'] = '填写你的空间名字';
{%endcodeblock%}

6.在discuz 管理中心->全局->上传设置->远程附件 中启用远程附件，并设置如下：
![](/img/2014/01/discuz-qiniu-01.jpg)
![](/img/2014/01/discuz-qiniu-01.jpg)

如上图，启用远程附件，FTP服务器填写你的七牛域名，账号密码为你的七牛账号和密码。被动模式选择默认。远程附件目录可以默认，也可以设置和本地目录一样`./data/attachment`,我设置了但是没效果，不知道为啥。远程访问URL设置你的七牛域名。扩展名就自己根据需要设置了。
