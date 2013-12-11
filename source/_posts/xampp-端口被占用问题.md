title: "Xampp 端口被占用问题"
id: 940
date: 2013-10-06 16:17:10
tags: 
- xampp 端口
- 端口占用
categories: 
- PHP5
---

<div id="zoomtext">ampp要修改两个地方才能启动Apache。不然就把模块Mod_SSL注释掉。就可以不用理443这个了。
XAMPP修改80和443端口</div><!-- more -->
<div></div>
<div>在启动XAMPP时，如果报80/443端口被占，可以修改此软件的端口</div>
<div>打开D:\Program Files\xampp\apache\conf\httpd.conf文件把80修改为8080；</div>
<div>打开D:\Program Files\xampp\apache\conf\extra\httpd-ssl.conf文件把443修改为4433或者关闭SSL扩展；</div>
<div></div>
<div>可以在命令行下输入“netstat -nab”查看当前端口使用情况.</div>
<div>XAMPP启动出现问题时，可以查看下列日志，帮助查找原因
D:\Program Files\xampp\apache\error下文件；D:\Program Files\xampp\apache\logs下文件。</div>
<div></div>