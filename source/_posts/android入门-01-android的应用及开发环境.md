title: Android入门-01-Android的应用及开发环境
date: 2013-12-26 13:36:51
categories: Android入门
tags: [Android入门,开发环境]
---
#**Android平台架构及特性**

Android系统的底层建立在Linux系统之上，该平台由`操作系统`、`中间件`、`用户界面`和`应用软件`4层组成，它采用一种被称为软件叠层(Software Stack)的方式进行构建。这种软件叠层结构使得层与层之间相互分离，明确各层分工。保证层与层之间的低耦合，当下层的层内或层下发送改变时，上层应用程序无须任何改变。
<!--more-->
其架构图如下：
![](/img/2014/01/android-goucheng-1.jpg)

##**Android系统组成**

- 应用层

  Android系统将会包含系列核心的应用程序，包括日历、浏览器、联系人等。

- 应用框架层

  Android系统最核心部分，集中体现了Android系统的设计思想。框架层由多个系统服务共同组成，包括【组件管理服务】，【窗口管理服务】，【地理信息服务】，【电源管理服务】，【通话管理服务】，等等。所有的服务都寄宿在 “系统核心进程中”，在运行时，每个服务都占据一个独立的线程，彼此通过进程间的通信机制发送消息和传输数据。

  对于开发者而言，框架层最直观的体现就是SDK，它通过一系列的Java功能模块，来实现应用所需要的功能。

- 核心类库

  核心类库由一系列的二进制动态库共同构成，通常使用C/C++进行开发。与框架层的系统服务相比，核心类库不能够独立运行于线程中，而需要被系统服务加载到其进程空间里，通过类库提供的JNI接口进行调用。

- Android运行时

  和所有的Java程序运行平台一样，为了实现Java程序在运行阶段的二次编译，Android为它们提供了运行时的支撑。
  Android运行时由Java核心类库和虚拟机Dalvik共同构成。
  JAVA核心类库涵盖了Android框架层和应用层所要用到的基础Java库，包括Android对象库，文件管理库，网络通信库等。
  Dalvik是为Android量身打造的Java虚拟机，负责动态解析执行应用，分配空间，管理对象生命周期等工作。

- Linux内核

  Android系统是建立在Linux 2.6之上。Linux内核提供了安全性、内存管理、进程管理、网络协议栈和驱动模型等核心系统服务。Linux内核是系统硬件和软件叠层之间的抽象层。

##**Android核心功能模块**

- 界面框架

  资源(Resource)和布局（Layout）体系，通过完善的控件库和接口设计，开发者可以搭建自己需要的界面。

- 数据存储

  本地存储和网络存储

- 网络通讯

  系统负责底层网络的连接和管理，开发者直接通过HTTP或Socket与远端服务器建立连接，而不需要关心是通过GPRS,3G还是WIFI来建立的。

- 地理信息

  基于GPS定位；通过网络利用基站信息进行定位；

- 图形和多媒体处理

  Android支持MPEG4，H.264，MP3，AAC，JPG，GIF等主流图像和音频格式。Android的音频处理主要依托于开源的OpenCORE项目。
  Android中对2D图形的使用，主要经由drawable包实现。在3D方面，Android搭配了OpenGL ES。

- 外部设备

  Android可以兼容各类输入设备，包括各种键盘，触摸屏，轨迹球等。

- 特色功能模块

  Android有统一的账号管理系统，当用户登录到Android系统后，Android中的其他应用便可以利用这些账号信息进行认证。统一的账号系统避免了用户注册和登录的麻烦，为开发者提供了新的机会。

  Android还有全局的事件通知机制--Notification。当应用需要将消息即时推送给用户时，可以利用这个对象，将消息发送到系统的状态栏中，并利用声音，震动，图标等方式提醒用户。

##**SDK文件夹介绍**

- `add-ons` 这里面保存着附加库，比如google Maps，当然你如果安装了OPhone SDK，这里也会有一些类库在里面。

- `docs` 这里面是Android SDK API参考文档，所有的API都可以在这里查到。

- `market_licensing` 作为Android Market版权保护组件，一般发布付费应用到电子市场可以用它来反盗版。

- `platforms` 是每个平台的SDK真正的文件，里面会根据API Level划分的SDK版本，  这里就以Android 2.2来说，进入后有一个android-8的文件夹，android-8进入后是Android 2.2 SDK的主要文件，其中ant为ant编译脚本，data保存着一些系统资源，images是模拟器映像文件，skins则是Android模拟器的皮肤，templates是工程创建的默认模板，android.jar则是该版本的主要framework文件，tools目录里面包含了重要的编译工具，比如aapt、aidl、逆向调试工具dexdump和编译脚本dx。

- `platform-tools` 保存着一些通用工具，比如adb、和aapt、aidl、dx等文件，Android123提示，这里和platforms目录中tools文件夹有些重复，主要是从android 2.3开始这些工具被划分为通用了。

- `amples` 是Android SDK自带的默认示例工程，里面的apidemos强烈推荐初学者运行学习，对于SQLite数据库操作可以查看NotePad这个例子，对于游戏开发 Snake、LunarLander都是不错的例子，对于Android主题开发Home则是android m5时代的主题设计原理。

- `tools` 作为SDK根目录下的tools文件夹，这里包含了重要的工具，比如ddms用于启动Android调试工具，比如logcat、屏幕截图和文件管理器，而draw9patch则是绘制android平台的可缩放png图片的工具，sqlite3可以在PC上操作SQLite数据库，而 monkeyrunner则是一个不错的压力测试应用，模拟用户随机按键，mksdcard则是模拟器SD映像的创建工具，emulator是 android模拟器主程序，不过从android 1.5开始，需要输入合适的参数才能启动模拟器，traceview作为android平台上重要的调试工具。

- `usb_driver` 顾名思义，保存着android平台google官方机型的驱动如nexus one、nexus s，同时也有一些老机型驱动的支持，比如说htc dream、htc magic和Motorola 的droid。