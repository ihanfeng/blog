title: "Mac OSX Eclipse启动时报需要安装\"Java SE 6 Runtime\"致无法启动解决方案"
id: 978
date: 2013-10-27 16:33:11
tags: 
categories: 
- linux
---

这周末有时间把黑苹果从10.8.5升级到最新版本的10.9 ，不想在启动Eclipse的时候报错，这是个坑爹的错误。我系统明明已经安装了JDK7，按理不理该出错的吧。

通过不断的Google终于找到了答案，系统本来是自带JDK1.6的，升级了居然被删除了，也就是不在带有1.6的版本。所以自己在官网下载最新版本安装，问题得以解决。

下载地址：[http://support.apple.com/kb/DL1572?viewlocale=zh_CN](http://support.apple.com/kb/DL1572?viewlocale=zh_CN "http://support.apple.com/kb/DL1572?viewlocale=zh_CN")