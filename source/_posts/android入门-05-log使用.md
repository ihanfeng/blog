title: Android入门-05-Log使用
date: 2013-12-26 15:08:18
categories: Android入门
tags: [Android入门,Log使用]
---
##**什么是Log**

Android Log是Android提供的日志工具类。

当我们在进行程序调试的时候，可以通过日志工具快速找到问题所在。Android提供的工具类是`android.util.Log`,类似JAVA开发中使用的Log4j。

android.util.Log常用的方法有以下5个：`Log.v()`、`Log.d()`、`Log.i()`、`Log.w()` 以及 `Log.e()` 。根据首字母对应`VERBOSE`，`DEBUG`,`INFO`, `WARN`，`ERROR`。
<!--more-->

1、**Log.v** 的调试颜色为黑色的，任何消息都会输出，这里的v代表verbose啰嗦的意思，平时使用就是Log.v("","");

2、**Log.d**的输出颜色是蓝色的，仅输出debug调试的意思，但他会输出上层的信息，过滤起来可以通过DDMS的Logcat标签来选择.

3、**Log.i**的输出为绿色，一般提示性的消息information，它不会输出Log.v和Log.d的信息，但会显示i、w和e的信息

4、**Log.w**的意思为橙色，可以看作为warning警告，一般需要我们注意优化Android代码，同时选择它后还会输出Log.e的信息。

5、**Log.e**为红色，可以想到error错误，这里仅显示红色的错误信息，这些错误就需要我们认真的分析，查看栈的信息了。


##**如何使用**

启动Eclipse,在Window->Show View会出来一个对话框，当我们点击Ok按钮时，会在控制台窗口出现LogCat视窗。
![](/img/2014/01/android-log-01.png)

LogCat工具加入后的效果如下图所示：
![](/img/2014/01/android-log-02.png)

具体使用：
Android  Log提供添加以上调试信息对应的方法 

{%codeblock lang:java%}
Log.v(String tag, String msg); //VERBOSE 
Log.d(String tag, String msg); //DEBUG 
Log.i(String tag, String msg); //INFO 
Log.w(String tag, String msg); //WARN 
Log.e(String tag, String msg); //ERROR 
{%endcodeblock%}

Tag为调试信息标签名称，msg为添加的调试信息。


