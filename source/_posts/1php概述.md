title: 1.PHP概述
date: 2013-12-11 12:23:22
categories: PHP5
tags: [php5,php学习]
---
##**PHP简介：**

`PHP`，是英文超级文本预处理语言`Hypertext Preprocessor`的缩写。PHP 是一种`HTML`内嵌式的语言，是一种在服务器端执行的嵌入HTML文档的脚本语言，语言的风格有类似于C语言，被广泛的运用。PHP 独特的语法混合了 C、Java、Perl 以及 PHP 自创的语法。它可以比 CGI或者Perl更快速的执行动态网页。用PHP做出的动态页面与其他的编程语言相比，PHP是将程序嵌入到HTML文档中去执行，执行效率比完全生成HTML标记的CGI要高许多；PHP还可以执行编译后代码，编译可以达到加密和优化代码运行，使代码运行更快。PHP具有非常强大的功能，所有的CGI的功能PHP都能实现，而且支持几乎所有流行的数据库以及操作系统。最重要的是PHP可以用C、C++进行程序的扩展！
<!-- more -->
##**PFP的发展：**

PHP原始为`Personal Home Page`的缩写，现已经正名为 "`PHP: Hypertext Preprocessor`"的缩写，注意不是“`Hypertext Preprocessor`”的缩写，这种将名称放到定义中的写法被称作递归缩写。PHP于19 94年由Rasmus Lerdorf创建，刚刚开始是Rasmus Lerdorf 为了要维护个人网页而制作的一个简单的用Perl语言编写的程序。最初这些工具程序用来显示 Rasmus Lerdorf 的个人履历，以及统计网页流量。后来又用C语言重新编写，包括可以访问数据库。他将这些程序和一些表单直译器整合起来，称为 PHP/FI。PHP/FI 可以和数据库连接，产生简单的动态网页程序。在1995年早期以Personal Home Page Tools (PHP Tools) 开始对外发表第一个版本，Lerdorf写了一些介绍此程序的文档，并且发布了PHP1.0。在这早期的版本中，提供了访客留言本、访客计数器等简单的功能。

以后越来越多的网站使用了PHP，并且强烈要求增加一些特性，比如循环语句和数组变量等等，在新的成员加入开发行列之后，Rasmus Lerdorf 在1995年6月8日将 PHP/FI 公开发布，希望可以透过社群来加速程序开发与寻找错误。这个发布的版本命名为 PHP 2，已经有今日 PHP 的一些雏型，像是类似 Perl 的变量命名方式、表单处理功能、以及嵌入到 HTML 中执行的能力。程序语法上也类似 Perl，有较多的限制，不过更简单、更有弹性。PHP/FI加入了对mySQL的支持，从此建立了PHP在动态网页开发上的地位。到了1996年底，有15000个网站使用 PHP/FI；在1997年，任职于 Technion IIT 公司的两个以色列程序设计师：Zeev Suraski 和 Andi Gutmans，重写了 PHP 的剖析器，成为 PHP 3 的基础，而 PHP 也在这个时候改称为PHP: Hypertext Preprocessor.[5]。

经过几个月测试，开发团队在1997年11月发布了 PHP/FI 2，随后就开始 PHP 3 的开放测试，最后在1998年6月正式发布 PHP 3。Zeev Suraski 和 Andi Gutmans 在PHP3 发布后开始改写 PHP 的核心，这个在1999年发布的剖析器称为 Zend Engine[7]，他们也在以色列的 Ramat Gan 成立了 Zend Technologies 来管理 PHP 的开发。在2000年5月22日，以Zend Engine 1.0为基础的PHP 4正式发布，2004年7月13日则发布了PHP 5，PHP 5则使用了第二代的Zend Engine[5]。PHP包含了许多新特色，像是强化的面向对象功能、引入PDO（PHP Data Objects，一个存取数据库的延伸函数库）、以及许多效能上的增强。目前PHP 4已经不会继续更新，以鼓励用户转移到PHP 5。2008年PHP 5成为了PHP唯一的有在开发的PHP版本。将来的PHP 5.3将会加入Late static binding和一些其他的功能强化。PHP 6 的开发也正在进行中，主要的改进有移除register_globals、magic quotes 和 Safe mode的功能。

##**PHP的特性：**

PHP的特性包括：

1.开放的源代码：所有的PHP源代码事实上都可以得到。
2.PHP是免费的。和其它技术相比，PHP本身免费。
3.php的快捷性 程序开发快，运行快，技术本身学习快。嵌入于HTML：因为PHP可以被嵌入于HTML语言，它相对于其他语言，编辑简单，实用性强，更适合初学者。
4.跨平台性强：由于PHP是运行在服务器端的脚本,可以运行在UNIX、LINUX、WINDOWS下。
5.效率高：PHP消耗相当少的系统资源。
6.图像处理：用PHP动态创建图像。
7.面向对象：在php4,php5 中，面向对象方面都有了很大的改进，现在php完全可以用来开发大型商业程序。
8.专业专注：PHP支持脚本语言为主，同为类C语言。