title: "magento之Magento 503 Service Temporarily Unavailable错误"
id: 605
date: 2013-06-04 16:46:03
tags: magento 503
categories: 
- magento
---

在安装一个插件失败后，访问magento出现错误如下

Error 503: Service Unavailable
<div>

# Service Temporarily Unavailable

</div>
The server is temporarily unable to service your request due to maintenance downtime or capacity problems. Please try again later.

清空缓存文件，删除local来解决问题未果。
<!-- more -->

于是我google了一下，找到了相关介绍，

如此帖子讲的：http://www.magentocommerce.com/boards/viewthread/24963/

说magento在一些情况下会生成一个 maintenance.flag 文件在根目录，来中断用户的访问，叫做进入了“maintenance mode”模式。只要删除根目录的maintenance.flag文件即可。