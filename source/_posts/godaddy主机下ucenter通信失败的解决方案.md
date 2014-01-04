title: Godaddy主机下Ucenter通信失败的解决方案
date: 2014-01-02 11:07:05
categories: Discuz
tags: [godaddy,通信失败,ucenter]
---
如果广大站长使用的是Godaddy的主机，那么不论是全新安装DZ+Ucenter，还是升级，99%会遇到应用通信失败的案例！
我是在本地把discuz X3 迁移到godaddy的服务器上，所以出现通信失败的案例。

我的版本是：Dz X3.1

找到UCenter安装目录下的`/model/misc.php`文件，找到这句，有两句，是第二处

	$out .= "Host: $host:$port\r\n";

替换为

	$out .= "Host: $host\r\n";

重新查看应用列表，即显示通信成功！

