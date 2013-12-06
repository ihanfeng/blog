title: "magento之实现后台登陆自动选择中文界面"
id: 593
date: 2013-06-04 15:08:35
tags: 
- magento
- 中文显示
- 后台
categories: 
- magento
---

在默认情况下，后台的界面使用的是英文界面，每次登陆后台我们需要手动去修改。这样比较繁琐。我们可以通过修改后台代码达到默认使用中文。

打开/app/code/core/Mage/Adminhtml/controllers/indexController.php文件

找到loginAction()函数，在函数里面添加一行代码：
<pre>Mage::getSingleton('adminhtml/session')-&gt;setLocale('zh_CN');</pre>
这样每次登陆后台时，系统都将默认选择中文界面。