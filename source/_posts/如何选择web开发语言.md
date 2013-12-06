title: "如何选择WEB开发语言"
id: 489
date: 2012-10-04 17:09:59
tags: 
- web开发
- 开发语言
- 语言选择
categories: 
- 程序人生
---

<span style="font-family: 华文中宋; font-size: small;">在打算开发一个网站时，选择什么语言，是首先需要面对的问题。目前主流的WEB开发语言有ASP.NET、PHP、JSP; 作为MS上世纪老将ASP，就不再提及，如果是因为维护方面的原因而必须使用，可考虑升级到ASP.NET，而作为新开发一个语言，实在找不到理由再使用它了；</span>
<!-- more -->
<span style="font-family: 华文中宋; font-size: small;">以下将对这三种语言做对比，以供权衡：</span>

**<span style="font-family: 华文中宋; font-size: small;">上手度</span>**

<span style="font-family: 华文中宋; font-size: small;">.NET: 5分</span>

<span style="font-family: 华文中宋; font-size: small;">PHP:3分</span>

<span style="font-family: 华文中宋; font-size: small;">JSP:1分</span>

<span style="font-family: 华文中宋; font-size: small;">如果你是一个WEB方面的新手，这三门WEB语言的学习成本差别很大。ASP.net 作为微软的产品，继承了其一贯的特点，方便上手，易用；甚至你都不用编码，靠着鼠标拖拖拽拽，都能整一个网站出来（网上，就有这样的视频讲解。当然，这样出来的网站是没法应用到实际中的，且不说其代码复用率极其低下，拖拽出来的代码，灵活度太小，效率也低（eg：gridview中的分页实现载入数据是一次全部载入的））。同时，凭着其强大的开发工具visual studio系列，在程序出现bug时，能最大程度的提供问题说明，让开发者尽快定位到问题所在。JSP相比而言难度就大多了，光是配置一个开发环境就得耗费不少精力，JSP语言最为头疼的就是程序调试方面，当程序出现问题时，并不能得到友好的错误提示，调试BUG比较耗时。再就是JSP依托的JAVA过于庞大，着实是个无底洞，开始容易，越往后发现要学的越多，一般互联网公司，还真难以有几个能驾驭，再普及的；PHP学习算是基于.net和JSP之间，语法与C语言一脉相承，上手也算容易；</span>

&nbsp;

**<span style="font-family: 华文中宋; font-size: small;">资源</span>**

<span style="font-family: 华文中宋; font-size: small;">.NET:4分</span>

<span style="font-family: 华文中宋; font-size: small;">PHP:5分</span>

<span style="font-family: 华文中宋; font-size: small;">JSP:2分</span>

<span style="font-family: 华文中宋; font-size: small;">资源包括能获取到的学习资料、开放源码，以及各种插件和库。PHP在这方面遥遥领先，粗略看来，各种网站的知名开源产品，大都使用PHP实现，如博客wordpress、论坛discuz、Wiki知识库MediaWiki等；</span>

<span style="font-family: 华文中宋; font-size: small;">相应的各种插件、库、开源代码的数量和质量更是其它语言无法相比。.NET资源也比较丰富，选用.NET幸福的是有MS这么一个强大后台做有力的技术支持，CSDN 的资料不但多，质量更是上乘；JSP由于其门槛高的缘故，致使在这方面的资料也比较少；</span>

&nbsp;

**<span style="font-family: 华文中宋; font-size: small;">系统架构实施</span>**

<span style="font-family: 华文中宋; font-size: small;">.NET:3分</span>

<span style="font-family: 华文中宋; font-size: small;">PHP:5分</span>

<span style="font-family: 华文中宋; font-size: small;">JSP: 3分</span>

<span style="font-family: 华文中宋; font-size: small;">.NET部署环境是windows 03/08+MS SQL Server + IIS。都是微软的产品，优点就是部署容易，方便，兼容性好。最为头疼就是安全方面的问题，windows下总是得不停的打补丁，但还是时常遭受这样那样的攻击；再就是数据库方面，MS SQL 与Oracle在并发处理、效率上始终有个数据量级的差距，2008发布之后据说是好了些，但总是让人感觉不大放心；PHP就是LAMP架构，即Linux+Apache+My Sql + PHP；Linux平台在我这几年的熟悉后，深刻体会到其就是为服务器而生，各种的工具让人爱不释手；My Sql作为开源产品，首先在软件费用上就公司能省下一大笔，其性能优秀，即使某日网站规模的扩大致使数据库出现瓶颈，也可组建一个数据库团队来研究改进。不过，在Oracle收购MySql之后，为其前景蒙上了一层阴影。有可能，在不久的将来，MySql的部分功能就会闭源。JSP的架构小则是Linux+apache+tomcat+MySql ,大则Linux + Apache + Java (WebSphere) + Oracle，对于一般小型网站的部署，大都选用第一种；WebSphere过于庞大，一般部署都得独自占用一台服务器；Oracle是数据库中的王者，性能优异（国内银行证券的数据库应用，一般只有DB2和Oracle两种选择），但其价格不菲，非一般创业公司能够承担（按CPU收费，一般25w/cpu/每年；次年会收取15%的维护费）需要提一下的是JSP系统架构部署有些难度，架构出现问题后，排错是个很痛苦的过程。</span>

**<span style="font-family: 华文中宋; font-size: small;">管理维护</span>**

<span style="font-family: 华文中宋; font-size: small;">.NET:2分</span>

<span style="font-family: 华文中宋; font-size: small;">PHP:5分</span>

<span style="font-family: 华文中宋; font-size: small;">JSP: 4分</span>

<span style="font-family: 华文中宋; font-size: small;">WEB管理中，经常会通过远程来管理网站，远程管理的方便与否关键看命令行工具的支持力度及脚本环境的操作便捷性。.NET只能跑在Windows平台上，远程管理一般只能通过图形化界面远程鼠标操作，当网速比较慢的时候，管理员的心情无比复杂，远程操作基本上是在一幅幅图片上估计下一张图片中鼠标的移动位置；Windows平台的命令行环境非常差，IIS的命令行工具功能少，bat脚本也难学难用（虽然可以通过安装cygwin工具来模拟linux shell环境，但系统操作，系统资源监控方面还是无能为力）； Linux下就幸福多了，远程基本上都是通过SSH连接，安全有保证，shell脚本消耗的网络带宽也只是图形化界面的百分之一，管理流畅，心情舒畅；各种程序消耗资源都可远程监控；Linux就是为服务器而生，此话毫不为过。PHP、JSP都可跨平台，一般其系统部署都是在Linux下，MySql数据库和apche服务器都可通过相应的命令行工具有效管理。JSP的应用服务器在这方面支持要少些；</span>

**<span style="font-family: 华文中宋; font-size: small;">跨平台</span>**

<span style="font-family: 华文中宋; font-size: small;">.NET:0 分</span>

<span style="font-family: 华文中宋; font-size: small;">PHP：5分</span>

<span style="font-family: 华文中宋; font-size: small;">JSP：5分</span>

<span style="font-family: 华文中宋; font-size: small;">曾几何时，我对跨平台不屑一顾，想着好端端的一个应用，既然是定位在这个平台上开发的，干嘛要移植到其它平台上。如今，我是深有体会。手上一个项目，公司由于成本压力，需要将应用从 SUN Unix移植到Linux平台（Redhat)。我们的程序基本上不用改动，在Linux上编译就只多了几个警告，改改就可上线了；而另一个项目，我被深度套牢！我们使用的是Windows平台的ASP.NET，由于受到Windows的病毒泛滥加上WEB管理的麻烦，迫切希望能移植到Linux平台，但这基本上不可能实现。若真想将这应用移植，只有下狠心使用PHP等重写应用，换系统架构。PHP、JSP都可跨平台，不用多说。</span>

**<span style="font-family: 华文中宋; font-size: small;">当前主流应用的选择</span>**

<span style="font-family: 华文中宋; font-size: small;">PHP：当前WEB创业公司的语言选择主要集中在PHP。除了上述原因还有一个重要原因就是PHP开发程序员队伍的规模。</span>

<span style="font-family: 华文中宋; font-size: small;">淘宝网（阿里巴巴）: Linux操作系统 + Web 服务器: Apache +PHP</span>

<span style="font-family: 华文中宋; font-size: small;">PHP的应用太多，这里不再列举；</span>

<span style="font-family: 华文中宋; font-size: small;">ASP.NET：在创业公司中应用不多，知名互联网应用有限，目前比较知名的应用有：</span>

<span style="font-family: 华文中宋; font-size: small;">博客园、CSDN、eBay、MySpace等；</span>

<span style="font-family: 华文中宋; font-size: small;">JSP：JSP实施比较庞大，用好的就得用到websphere或weblogic这样的大物件，种种原因使得JSP在互联网公司中应用并不多，除了阿里巴巴，没有几个公司能驾驭JAVA（JSP）。深入JAVA需要多年修炼，而成精之后，公司是否有足够的薪水来留住这么一群高手是个考验；</span>

<span style="font-family: 华文中宋; font-size: small;">阿里巴巴：Linux+（JSP）</span>

**<span style="font-family: 华文中宋; font-size: small;">总结</span>**

<span style="font-family: 华文中宋; font-size: small;">如今流行的Ruby，也是创业公司的一个选择；python的优雅，也可考虑尝试（豆瓣使用的Python）；但选择这些语言的一个风险是公司规模扩大后，是否能找到足够的人才得打个问号。总的来说，创业面临选择一门开发语言，PHP当是首选；如果不考虑Linux平台，铁定在Windows上运营，.NET也是一个不错的选择。JSP小公司勿近，危险，容易造成的资金套牢。</span>

&nbsp;

<span style="color: #c0c0c0;">作者：<span style="color: #ff0000;">[<span style="color: #ff0000;">大CC</span>](http://www.cnblogs.com/me115/archive/2011/09/27/2193250.html)</span></span>

<span style="color: #c0c0c0;">除非注明，文章均为<span style="color: #ff0000;">[<span style="color: #ff0000;">寒枫</span>](http://173.234.48.113 "寒枫")</span>原创，欢迎转载！转载请注明本文地址，谢谢。</span>

<span style="color: #c0c0c0;">本文地址：<span style="color: #ff0000;">[<span style="color: #ff0000;">http://173.234.48.113/如何选择web开发语言.html</span>](http://173.234.48.113/如何选择web开发语言.html)</span></span>