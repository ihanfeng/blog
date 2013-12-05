title: Hexo教程二：Hexo博客搭建
date: 2013-12-05 11:53:24
categories: hexo
tags: [hexo,hexo搭建]
---

##**1、安装Git**

windows下推荐安装[msysgit](http://msysgit.github.io/)，安装步骤详情Google。

##**2、安装Node.js**

官网：[http://nodejs.org/](http://nodejs.org/)

下载直接安装即可，不懂的自行google。

<!-- more -->
##**3、安装hexo**

利用`npm`命令即可安装。 任意位置右键选择`git bash`，输入如下命令：

	npm install -g hexo 

##**4、创建hexo文件夹**

hexo安装完成后，在E盘建立文件夹，命名为blog，然后执行命令。

	hexo init 

Hexo会自动在目标文件夹建立网站所需的所有文件。

##**5、本地查看**

现在我们已经在本地搭建完Hexo博客，执行以下命令即可在本地查看。

	hexo generate //生成博客文件

	hexo server // 启动服务


在浏览器输入`localhost:4000`即可查看。


##**6、注册Github和创建repository**

没有`Github`账号的请自行注册，我已经有账号，所有就不在注册。 在自己Github主页右下角，创建一个新的repository。比如我的Github账号是`zhdevelop`，那么我应该创建的repository名字应该是`zhdevelop.github.io`。


##**7、部署**

编辑`_config.yml`。

	# Deployment
	## Docs: http://zespia.tw/hexo/docs/deploy.html
	deploy:
  	type: github
  	repository: https://github.com/zhdevelop/zhdevelop.github.io.git
  	branch: master

执行以下指令完成部署。

	hexo generate
	hexo deploy


注意： 每次修改本地文件后，需要`hex generate`才能保存。每次使用命令都必须在自己创建的文件夹目录下，如e:\blog

这样我们的博客就已经搭建完毕了。直接访问`http://zhdevelop.github.io/` 即可。

##**8、tips**

hexo现在支持更加简单的命令格式了，比如：

	hexo g == hexo generate
	hexo d == hexo deploy
	hexo s == hexo server
	hexo n == hexo new



