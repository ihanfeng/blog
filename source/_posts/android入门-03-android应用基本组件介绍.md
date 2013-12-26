title: Android入门-03-Android应用基本组件介绍
date: 2013-12-26 14:13:42
categories: Android入门
tags: [Android入门,基本组件,四大组件]
---
Android应用通常由一个或多个基本组件组成，Activity组件是Android中最常用到的。

#**Activity和View**

`Activity`是Android应用中负责与用户交互的组件。通过`setContentView(View)`来显示指定组件。

View组件是所有UI控件、容器控件的基类，View组件就是Android应用中用户实实在在看到的部分。View组件需要放到容器组件中，或者使用Activity将它显示出。

`setContentView()`方法可以接受一个View对象作为参数。

Activity包含一个`setTheme(int resid)`方法来设置窗口的风格。
<!--more-->
#**Service**

Service与Activity地位并列，也代表一个单独的Android组件。`Service`通常位于后台运行，一般不需要与用户交互，隐藏没有图像用户界面。

Service组件需要继承Service基类。一个Service组件被运行起来之后，它将拥有自己独立的生命周期，Service组件通常用于为其他组件提供后台服务或监控其他组件的运行状态。

#**BroadcastReceiver**

`BroadcastReceiver`代表广播消息接收器。使用BroadcastReceiver组件接收广播消息比较简单，只要实现BroadcastReceiver子类，并重写onReceive(Context context,Intent intent)方法即可。当其他组件通过sendBroadcast()、sendStickyBroadcast()或者sendOrderBroadcast()方法发送广播消息时，如该BroadcastReceiver对该消息感兴趣，BroadcastReceiver的onReceive(Context context,Intent intent)方法将会被触发。

注册广播：
在java代码中通过Context.registReceiver()方法注册BroadcastReceiver
在AndroidManifest.xml 文件中使用`<receiver ../>`元素完成注册

#**ContentProvider**

Android系统为跨应用的数据交换提供一个标准：`ContentProvider`。当用户实现自己的ContentProvide时，需要实现如下抽象方法。

- insert(Uri,ContentValues):向ContentProvider插入数据
- delete(Uri,ContentValues):删除ContentProvider指定数据
- update(Uri,ContentValues,String,String[]);更新ContentProvider中指定数据
- query(Uri,String[],String,String[],string):从ContentProvider查询数据

一个应用程序使用ContentProvider暴漏自己的数据，另一个程序则通过ContentResolver来访问。

#**Intent和IntentFilter**

Android应用内不同组件之间通信的载体。当Android运行时需要连接不同的组件时，通常需要借助于Intent来实现。Intent可以启动应用中另一个Activity，也可以启动一个Service组件，还可以发送一条广播消息来触发系统中的BroadcastReceiver。

启动一个Activity时，调用Context的`startActivity(Intent intent)`或`startActivityForResult(Intent intent,int requestCode)`方法，Intent参数封装了需要启动的目标Activity的信息。

启动一个Service时，调用Context的`startService(Intent intent)`或`bindService(Intent intent,ServiceConnection conn,int flags)`方法，Intent参数封装了需要启动的目标Service的信息。

当需要触发一个BroadcastReceiver时，调用Context的`sendBroadcast(Intent intent)`、`sendStickyBroadcast(Intent intent)`或`sendOrderedBroadcast(Intent intent,String receiverPermission)`方法来发送广播消息。

- 显示Intent：显示Intent明确指定需要启动或者触发的组件的类名
- 隐式Intent：只是指定需要启动或触发组件应该满足怎么样的条件

