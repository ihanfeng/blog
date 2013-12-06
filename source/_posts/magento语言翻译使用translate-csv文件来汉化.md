title: "magento语言翻译,使用translate.csv文件来汉化"
id: 974
date: 2013-10-07 15:20:31
tags: 
categories: 
- magento
---

一般Magento的语言包都是指/app/locale目录下的文件夹，以中文包为例，/app/locale/zh_CN下的所有文件就是中文语言包的全部内容（具体可见从[http://www.magentochina.org/bbs/](http://www.magentochina.org/bbs/)下载的Magento汉化包）。

细心地人可能会发现，除了这里有csv文件，在模板文件目录下也有一个locale文件夹，这里同样有个文件名为translate.csv的csv文件（在各自语言文件夹下，比如默认在locale下就只有一个en_US文件夹，里面自带一个translate.csv文件）。
<!-- more -->
现在我们来做个实验，在你所使用的模板目录下/app/design/frontend/default/default/locale（这里以default为例），新建文件夹zh_CN，在这个文件夹下新建文件translate.csv，打开translate.csv，添加这样一句：

[xml]&lt;/p&gt;&lt;p&gt;&quot;Theme Design&quot;,&quot;主题设计&quot;&lt;/p&gt;&lt;p&gt;[/xml]

保存。

现在打开前台，你会发现原来的“Theme Design”变成了“主题设计”。

&nbsp;

可以推断出，translate.csv里的翻译要比/app/locale/下的语言文件里的翻译优先级要高。

 其实从这个文件放的位置就可以理解，这个csv文件是专门给所在的模板用的，当使用这个模板时，translate.csv里的翻译项会覆盖掉语言包里的同名项，至于实际用法，以上面的为例，国内的语言包现在都是把My Cart翻译成“我的购物车”，这个翻译没有问题，但如果是做一个服装网站，把它翻译成“购物袋”是不是会更讨巧和更有创意呢，这时你不需要去修改/app/locale/zh_CN目录下的文件，而是像上面的例子一样去translate.csv新增项来覆盖掉原来的。

以为自身的使用情况来说，虽然网上有现成的中文汉化包提供下载，但并没有做到百分百汉化（其中有一些是Magento自带的bug造成的），特别是后台，而国内的客户是很难接受在后台经常看到英文的，所以在这个汉化包的基础上，我经常需要把发现的漏网之鱼做好翻译并加到语言包里去，积累起来更完善的语言包以便下个项目可以重用，这时就会存在一个问题，有些项目的特殊性会要求把同一段英文翻译成不同的中文（还是以购物车和购物袋为例），如果把这一类的翻译直接去改语言包里的文件来实现，下一个项目要重用这个语言包就会带来问题。所以，把所有可能个性的，无法重用的翻译都写到translate.csv里去是一种正确和合理的思路，我觉得这也是官方提供这种方式的初衷。

PS：后台模板目录下同样存在这个文件，可以用同样的方式修改后台的翻译

&nbsp;