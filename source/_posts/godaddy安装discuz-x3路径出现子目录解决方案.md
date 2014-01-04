title: Godaddy安装discuz X3路径出现子目录解决方案
date: 2014-01-02 11:39:42
categories: Discuz
tags: [godaddy,discuz,X3,路径问题,子目录]
---
在Godaddy上安装discuz X3，因为是创建子目录，不是在根目录安装，所有安装后路径会出现子目录名字，看着不美观，所以就想办法去除掉，google好久都没发现可行的办法，都是相同的复制过去的答案，要么就是说windows主机问题，其实我的linux主机呢。就自己根据搜索的一些结果进行研究，发现其实很简单。
<!--more-->
查看dz的源代码（我的是dzx 3.1）发现base的值是由`$_G['siteurl']`这个变量决定的。

找了一下发现`$_G`数组的定义在`source/class/discuz/discuz_application.php`里面。

找到188行：

	$_G['siteurl'] = dhtmlspecialchars('http'.($_G['isHTTPS'] ? 's' : '').'://'.$_SERVER['HTTP_HOST'].$sitepath.'/');

更改为

	$_G['siteurl'] = dhtmlspecialchars('http'.($_G['isHTTPS'] ? 's' : '').'://'.$_SERVER['HTTP_HOST'].'/');

保存刷新既可以看到已经更改过来了。清爽多了有没有。