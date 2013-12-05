title: 使用Github管理Blog
date: 2013-12-05 16:19:46
categories: hexo
tags: [github,blog管理]
---
使用Github主要是考虑到不同电脑之间的数据同步，不然换台电脑，没有博客源程序，那就哭死都没用啦。

##**推送本地的Blog文件夹到github**

1.在github建立repositories，名字为命名为`blog`（你用hexo init的文件夹名）
2.在本地进入`blog`文件夹，`git init`建立仓库
3.建立`.gitignore`文件，里面填写`/public`（由于public每次generate都会更新，故此忽略pulic文件夹的git管理）<!-- more -->
4.用命令`git add .`告诉git，把所有文件添加到仓库
5.用命令`git commit -m "add all files"`把文件提交倒仓库
6.把本机仓库的内容推送到github仓库：

{%codeblock lang:html%}
git remote add origin git@github.com:zhdevelop/blog.git
git push -u origin master
{%endcodeblock%}

##**使用github管理hexo的source**
1.写博客后，`git add <file>`添加到代码库
2.提交当前修改作为一个记录：`git commit -m` 修改了<修改的文件>，原因是：……。
3.更新代码：`git push`
4.在另一电脑，`git clone <address>`：复制代码库到本地。
5.当远端github有更新时，`git pull`：从远程同步代码库到本地。

每次手工去输入git命令实在是麻烦，推荐大家使用[TortoiseGit](http://code.google.com/p/tortoisegit/)来管理。