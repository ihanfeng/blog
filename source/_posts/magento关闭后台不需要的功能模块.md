title: "magento关闭后台不需要的功能模块"
id: 953
date: 2013-10-06 19:47:54
tags: 
- 模块关闭
categories: 
- magento
---

**Magento** 功能确实非常强大，但有一些功能模块是我等所用不到的，可以考虑禁用以提高系统速度。

稍微熟悉Magento的人马上就会想到Magento的模块化标准，其实不论是前台或者后台都是通过一个一个模板中的一个个Block组织成你所看到的，至于用户诱发的动作有一部分是在controller中完成的，有的则是在model中实现的，还有些是直接放在block中的。
<!-- more -->
显然controller是负责指挥。

Block可以说是负责显示，或者说是指导模板如何显示，当然它也可以处理些数据，当然是读操作比较多。

model理论上讲，负责操作处理数据，但主要应该是写的操作。当然也有读的能力。

在Magento中所有模块的开关都是在app/etc/modules中的文件进行配置的，要把一个模块禁用，步骤如下：

1.  确定你要关闭的模块，比如我们这边要关闭的是后台的**Magento通知信息模块** ：AdminNotification
2.  到app/etc/modules目录下，找到包含这个模板定义的xml文件
3.  删掉它的相关定义，或将&lt;active&gt;true&lt;/active&gt;值改成false;
PS：最快捷的方法：

进入后台--&gt;System--&gt;Configuration--&gt;Advanced--&gt;这里可以直接关闭你不想要的功能模块。