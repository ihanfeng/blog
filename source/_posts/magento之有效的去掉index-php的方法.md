title: "magento之有效的去掉index.php的方法"
id: 603
date: 2013-06-04 16:16:15
tags: 
categories: 
- magento
---

开启重写URL 然后设置.htaccess。

.htaccess代码如下：

Options +FollowSymLinks

RewriteEngine On

RewriteBase /

RewriteCond %{REQUEST_URI} !^/media/
RewriteCond %{REQUEST_URI} !^/skin/
RewriteCond %{REQUEST_URI} !^/js/
RewriteCond %{REQUEST_URI} !^/var/

RewriteCond %{REQUEST_FILENAME} !-f

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-l

RewriteRule . index.php [L]