title: Android入门-02-Android 工程项目介绍
date: 2013-12-26 13:48:18
categories: Android入门
tags: [Android入门,项目工程]
---
#**Android工程目录结构**

首先通过一张新建的android项目图来展示下android工程架构：
![](/img/2014/01/android-gongcheng-01.jpg)
<!--more-->
现在来详细介绍每个目录的作用
![](/img/2014/01/android-gongcheng-02.jpg)
`/src`，即是Android项目的源代码，里面包含所创建的"activity"文件，这是一个java文件，该目录文件是根据package结构管理的。

![](/img/2014/01/android-gongcheng-03.jpg)
`/gen`这个也是源代码目录，但是它只包含android平台自动生成的Java源代码文件。截图中有个R类，是生成的Java文件中最重要的一个。android framework负责生成R类文件。相见[参考](http://developer.android.com/reference/android/R.html)

![](/img/2014/01/android-gongcheng-04.jpg)
`/res`
该目录主要存放Android项目的各种资源文件，如layout存放界面布局文件，values存放各种XML格式的资源文件，如字符串资源文件:string.xml;颜色资源文件：colors.xml；尺寸资源文件：dimens.xml。
drawable-ldpi：低分辨率图片；drawable-mdpi：中分辨率图片；drawable-hdpi：高分辨率图片；drawable-xhdpi：超高分辨率图片

`AndroidManifest.xml`
这个XML文件包含了android应用中的元信息，是每个android项目中的重要文件。它包含了activity（行为）、view（视图）、service（服务）之类的信息，以及运行这个android应用程序需要的用户权限列表，同时也详细描述了android应用的项目结构。参考

#**res目录重点说明**

Android按照约定，将不同的资源放在不同的文件夹内，这样可以方便让AAPT工具扫描，并生成对应的资源清单类：`R.java`

以/HelloWorld/res/values/strings.xml文件为例。
{%codeblock lang:xml%}
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">HelloWorld</string>
</resources>
{%endcodeblock%}
上面资源文件定义一个字符串常量，常量值为HelloWorld，常量名称为app_name。

- 在Java代码中使用资源

  R.java为每个资源分别定义一个内部类，其中每个资源项对应于内部类里的一个int类型的Field。
{%codeblock lang:java%}
public static final class string {
public static final int app_name=0x7f050000;
}
{%endcodeblock%}
  借助于AAPT自动生成的R类的帮助，在java代码中我们可以通过R.String.app_name来引用到"HelloWorld'字符串常量。

- 在XML文件中使用

	格式：@<资源对应的内部类的类名>/<资源项的名称>  如：@string/app_name

  当我们在XML文件中使用标识符时，这些标识符无须使用专门的资源进行定义，直接在xml文档中按如下格式分配标识符即可：`@+id/<标识符代号>`
	{%codeblock lang:xml%}
	android:id="@+id/ok"
	{%endcodeblock%}
  在java代码中获取该组件，通过调用Activity的findViewById()方法即可实现。


##**清单文件 AndroidManifest.xml**

每个Android项目所必须的，是整个Android应用的全局描述文件。AndroidManifest.xml清单文件说明了该应用的名称、所使用的图标以及包含的组件等。

清单包含如下信息：

- 应用程序的包名，该包名将会作为该应用的唯一标识。
- 应用程序包含的组件：如Activity、Service、BroadcastReceiver、ContentProvider等。
- 应用程序兼容的最低版本
- 应用程序使用系统所需的权限声明
- 其他程序访问该程序所需的权限声明

样例代码如下：
{%codeblock lang:xml%}
<?xml version="1.0" encoding="utf-8"?>
<!--
	package:表示整个java引用程序的主要包名，是一个默认的程序名称
	android:versionCode：表示工程所生成的apk版本号，1、2、3、....
	android:versionName： 表示版本的一个名称 1.0、2.0、...
	android:installLocation="auto": 自动寻找安装地方，ROM或SDcard卡
	internalOnly: 仅仅只能安装在ROM上
	preferExternal:直接安装在SDcard
-->
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.hanfeng.app.helloworld"
    android:versionCode="1"
    android:versionName="1.0" 
    android:installLocation="auto"
    >
    <uses-sdk
        android:minSdkVersion="8"
        android:targetSdkVersion="18" />
    <!-- 
    	指定Android应用标签、图标 
		android:icon="@drawable/ic_launcher"：应用程序的Logo图片
		android:label="@string/app_name"：工程文字说明
	-->
    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <!-- 
        定义android应用的一个组件:Activity 
	
		android:name 表示整个应用程序的主程序的名称
		intent-filter 意图过滤器：用来过滤用户的一些动作
		category android:name 决定应用程序是否在程序列表显示
    	-->
        <activity
            android:name="com.hanfeng.app.helloworld.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <!-- 指定该Activity是程序的入口 -->
                <action android:name="android.intent.action.MAIN" />
                <!-- 指定加载该应用时运行该Activity -->
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
{%endcodeblock%}

##**应用程序权限说明**

声明运行该应用本身所需要的权限

  通过`<manifest.../>`元素添加`<uses-permission../>` 资源是即可为程序本身声明权限。
  如：

{%codeblock lang:xml%}
  <uses-permission android:name="android.permission.CALL_PHONE"/>  //声明该应用本身需要打电话的权限
{%endcodeblock%}

声明调用该应用所需的权限
     
  通过为应用的各组件元素，如`<activity../>`元素添加`<uses-permission.../>`子元素即可声明调用该程序所需的权限。

  实际上，Android提供大量权限，可以参看
  [Mainfest.permission](http://developer.android.com/reference/android/Manifest.permission.html)
  类，常用的权限如下:

 <table border="1"><tr><td>属性</td><td>说明</td></tr><tr><td>android.permission.ACCESS_CHECKIN_PROPERTIES</td><td>允许读写访问&#160;"properties"表在checkin数据库中，改值可以修改上传</td></tr><tr><td>android.permission.ACCESS_COARSE_LOCATION</td><td>通过WiFi或移动基站的方式获取用户错略的经纬度信息，定位精度大概误差在30~1500米</td></tr><tr><td>android.permission.ACCESS_FINE_LOCATION</td><td>通过GPS芯片接收卫星的定位信息，定位精度达10米以内</td></tr><tr><td>android.permission.ACCESS_LOCATION_EXTRA_COMMANDS</td><td>允许应用程序访问额外的位置提供命令</td></tr><tr><td>android.permission.ACCESS_MOCK_LOCATION</td><td>获取模拟定位信息，一般用于帮助开发者调试应用</td></tr><tr><td>android.permission.ACCESS_NETWORK_STATE</td><td>获取网络信息状态，如当前的网络连接是否有效</td></tr><tr><td>android.permission.ACCESS_SURFACE_FLINGER</td><td>Android平台上底层的图形显示支持，一般用于游戏或照相机预览界面和底层模式的屏幕截图</td></tr><tr><td>android.permission.ACCESS_WIFI_STATE</td><td>允许程序访问Wi-Fi网络状态信息</td></tr><tr><td>android.permission.ACCOUNT_MANAGER</td><td>获取账户验证信息，主要为GMail账户信息，只有系统级进程才能访问的权限</td></tr><tr><td>android.permission.AUTHENTICATE_ACCOUNTS</td><td>允许一个程序通过账户验证方式访问账户管理ACCOUNT_MANAGER相关信息</td></tr><tr><td>android.permission.ADD_SYSTEM_SERVICE</td><td>允许程序发布系统级服务</td></tr><tr><td>android.permission.BATTERY_STATS</td><td>允许程序更新手机电池统计信息</td></tr><tr><td>android.permission.BIND_APPWIDGET</td><td>允许一个程序告诉appWidget服务需要访问小插件的数据库，只有非常少的应用才用到此权限</td></tr><tr><td>android.permission.BIND_DEVICE_ADMIN</td><td>请求系统管理员接收者receiver，只有系统才能使用</td></tr><tr><td>android.permission.BIND_INPUT_METHOD</td><td>请求InputMethodService服务，只有系统才能使用</td></tr><tr><td>android.permission.BIND_REMOTEVIEWS</td><td>必须通过RemoteViewsService服务来请求，只有系统才能用</td></tr><tr><td>android.permission.BIND_WALLPAPER</td><td>必须通过WallpaperService服务来请求，只有系统才能用</td></tr><tr><td>android.permission.BLUETOOTH</td><td>允许程序连接到已配对的蓝牙设备</td></tr><tr><td>android.permission.BLUETOOTH_ADMIN</td><td>允许程序发现和配对蓝牙设备</td></tr><tr><td>android.permission.BRICK</td><td>能够禁用手机，非常危险，顾名思义就是让手机变成砖头</td></tr><tr><td>android.permission.BROADCAST_PACKAGE_REMOVED</td><td>允许程序广播一个提示消息在一个应用程序包已经移除后</td></tr><tr><td>android.permission.BROADCAST_STICKY</td><td>允许一个程序广播常用intents</td></tr><tr><td>android.permission.CALL_PHONE</td><td>允许一个程序初始化一个电话拨号不需通过拨号&#160;用户界面需要用户确认</td></tr><tr><td>android.permission.CALL_PRIVILEGED</td><td>允许一个程序拨打任何号码，包含紧&#160;急号码无需通过拨号用户界面需要用户确认</td></tr><tr><td>android.permission.CAMERA</td><td>请求访问使用照相设备</td></tr><tr><td>android.permission.CHANGE_COMPONENT_ENABLED_STATE</td><td>改变组件是否启用状态</td></tr><tr><td>android.permission.CHANGE_CONFIGURATION</td><td>允许一个程序修改当前设置，如本地化</td></tr><tr><td>android.permission.CHANGE_NETWORK_STATE</td><td>改变网络状态如是否能联网</td></tr><tr><td>android.permission.CHANGE_WIFI_MULTICAST_STATE</td><td>改变WiFi多播状态</td></tr><tr><td>android.permission.CHANGE_WIFI_STATE</td><td>允许程序改变Wi-Fi连接状态</td></tr><tr><td>android.permission.CLEAR_APP_CACHE</td><td>清除应用缓存</td></tr><tr><td>android.permission.CLEAR_APP_USER_DATA</td><td>清除应用的用户数据</td></tr><tr><td>android.permission.CWJ_GROUP</td><td>允许CWJ账户组访问底层信息</td></tr><tr><td>android.permission.CELL_PHONE_MASTER_EX</td><td>手机优化大师扩展权限</td></tr><tr><td>android.permission.CONTROL_LOCATION_UPDATES</td><td>允许获得移动网络定位信息改变</td></tr><tr><td>android.permission.DELETE_CACHE_FILES</td><td>允许程序删除缓存文件</td></tr><tr><td>android.permission.DELETE_PACKAGES</td><td>允许程序删除应用</td></tr><tr><td>android.permission.DEVICE_POWER</td><td>允许访问底层电源管理</td></tr><tr><td>android.permission.DIAGNOSTIC</td><td>允许程序RW诊断资源</td></tr><tr><td>android.permission.DISABLE_KEYGUARD</td><td>允许程序禁用键盘锁</td></tr><tr><td>android.permission.DUMP</td><td>允许程序返回状态抓取信息从系统服务</td></tr><tr><td>android.permission.EXPAND_STATUS_BAR</td><td>允许一个程序扩展收缩在状态栏,Android开发网提示应该是一个类似Windows&#160;Mobile中的托盘程序</td></tr><tr><td>android.permission.FACTORY_TEST</td><td>作为一个工厂测试程序，运行在root用户</td></tr><tr><td>android.permission.FLASHLIGHT</td><td>访问闪光灯,Android开发网提示HTC&#160;Dream不包含闪光灯</td></tr><tr><td>android.permission.FORCE_BACK</td><td>允许程序强行一个后退操作是否在顶层activities</td></tr><tr><td>android.permission.FOTA_UPDATE</td><td>暂时不了解这是做什么使用的，Android开发网分析可能是一个预留权限.</td></tr><tr><td>android.permission.GET_ACCOUNTS</td><td>访问GMail账户列表</td></tr><tr><td>android.permission.GET_PACKAGE_SIZE</td><td>允许一个程序获取任何package占用空间容量</td></tr><tr><td>android.permission.GET_TASKS</td><td>允许一个程序获取信息有关当前或最近运行的任务，一个缩略的任务状态，是否活动等等</td></tr><tr><td>android.permission.GLOBAL_SEARCH</td><td>允许程序使用全局搜索功能</td></tr><tr><td>android.permission.HARDWARE_TEST</td><td>访问硬件辅助设备，用于硬件测试</td></tr><tr><td>android.permission.INJECT_EVENTS</td><td>允许一个程序截获用户事件如按键、触&#160;摸、轨迹球等等到一个时间流</td></tr><tr><td>android.permission.INSTALL_LOCATION_PROVIDER</td><td>安装定位提供</td></tr><tr><td>android.permission.INSTALL_PACKAGES</td><td>允许程序安装应用</td></tr><tr><td>android.permission.INTERNAL_SYSTEM_WINDOW</td><td>允许程序打开内部窗口，不对第三方应用程序开放此权限</td></tr><tr><td>android.permission.INTERNET</td><td>访问网络连接，可能产生GPRS流量</td></tr><tr><td>android.permission.KILL_BACKGROUND_PROCESSES</td><td>允许程序调用killBackgroundProcesses(String).方法结束后台进程</td></tr><tr><td>android.permission.MANAGE_ACCOUNTS</td><td>允许程序管理AccountManager中的账户列表</td></tr><tr><td>android.permission.MANAGE_APP_TOKENS</td><td>管理创建、摧毁、Z轴顺序，仅用于系统</td></tr><tr><td>android.permission.MTWEAK_USER</td><td>允许mTweak用户访问高级系统权限</td></tr><tr><td>android.permission.MTWEAK_FORUM</td><td>允许使用mTweak社区权限</td></tr><tr><td>android.permission.MASTER_CLEAR</td><td>允许程序执行软格式化，删除系统配置信息</td></tr><tr><td>android.permission.MODIFY_AUDIO_SETTINGS</td><td>修改声音设置信息</td></tr><tr><td>android.permission.MODIFY_PHONE_STATE</td><td>修改电话状态，如飞行模式，但不包含替换系统拨号器界面</td></tr><tr><td>android.permission.MOUNT_FORMAT_FILESYSTEMS</td><td>格式化可移动文件系统，比如格式化清空SD卡</td></tr><tr><td>android.permission.MOUNT_UNMOUNT_FILESYSTEMS</td><td>挂载、反挂载外部文件系统</td></tr><tr><td>android.permission.NFC</td><td>允许程序执行NFC近距离通讯操作，用于移动支持</td></tr><tr><td>android.permission.PERSISTENT_ACTIVITY</td><td>创建一个永久的Activity，该功能标记为将来将被移除</td></tr><tr><td>android.permission.PROCESS_OUTGOING_CALLS</td><td>允许程序监视，修改或放弃播出电话</td></tr><tr><td>android.permission.READ_CALENDAR</td><td>允许程序读取用户的日程信息</td></tr><tr><td>android.permission.READ_CONTACTS</td><td>允许应用访问联系人通讯录信息</td></tr><tr><td>android.permission.READ_FRAME_BUFFER</td><td>读取帧缓存用于屏幕截图</td></tr><tr><td>com.android.browser.permission.READ_HISTORY_BOOKMARKS</td><td>读取浏览器收藏夹和历史记录</td></tr><tr><td>android.permission.READ_INPUT_STATE</td><td>读取当前键的输入状态，仅用于系统</td></tr><tr><td>android.permission.READ_LOGS</td><td>允许程序读取底层系统日志文件</td></tr><tr><td>android.permission.READ_PHONE_STATE</td><td>访问电话状态</td></tr><tr><td>android.permission.READ_OWNER_DATA</td><td>允许程序读取所有者数据</td></tr><tr><td>android.permission.READ_SMS</td><td>允许程序读取短信息</td></tr><tr><td>android.permission.READ_SYNC_SETTINGS</td><td>读取同步设置，读取Google在线同步设置</td></tr><tr><td>android.permission.READ_SYNC_STATS</td><td>读取同步状态，获得Google在线同步状态</td></tr><tr><td>android.permission.REBOOT</td><td>允许程序重新启动设备</td></tr><tr><td>android.permission.RECEIVE_BOOT_COMPLETED</td><td>允许程序开机自动运行</td></tr><tr><td>android.permission.RECEIVE_MMS</td><td>接收彩信</td></tr><tr><td>android.permission.RECEIVE_SMS</td><td>接收短信</td></tr><tr><td>android.permission.RECEIVE_WAP_PUSH</td><td>接收WAP&#160;PUSH信息</td></tr><tr><td>android.permission.RECORD_AUDIO</td><td>录制声音通过手机或耳机的麦克</td></tr><tr><td>android.permission.REORDER_TASKS</td><td>允许程序改变Z轴排列任务</td></tr><tr><td>android.permission.RESTART_PACKAGES</td><td>结束任务通过restartPackage(String)方法，该方式将在外来放弃</td></tr><tr><td>android.permission.SEND_SMS</td><td>发送短信</td></tr><tr><td>android.permission.SET_ACTIVITY_WATCHER</td><td>设置Activity观察器一般用于monkey测试</td></tr><tr><td>com.android.alarm.permission.SET_ALARM</td><td>设置闹铃提醒</td></tr><tr><td>android.permission.SET_ALWAYS_FINISH</td><td>设置程序在后台是否总是退出</td></tr><tr><td>android.permission.SET_ANIMATION_SCALE</td><td>设置全局动画缩放</td></tr><tr><td>android.permission.SET_DEBUG_APP</td><td>设置调试程序，一般用于开发</td></tr><tr><td>android.permission.SET_ORIENTATION</td><td>设置屏幕方向为横屏或标准方式显示，不用于普通应用</td></tr><tr><td>android.permission.SET_PREFERRED_APPLICATIONS</td><td>设置应用的参数，已不再工作具体查看addPackageToPreferred(String)&#160;介绍</td></tr><tr><td>android.permission.SET_PROCESS_FOREGROUND</td><td>允许程序当前运行程序强行到前台</td></tr><tr><td>android.permission.SET_PROCESS_LIMIT</td><td>允许程序设置最大的进程数量的限制</td></tr><tr><td>android.permission.SET_TIME</td><td>设置系统时间</td></tr><tr><td>android.permission.SET_TIME_ZONE</td><td>设置系统时区</td></tr><tr><td>android.permission.SET_WALLPAPER</td><td>允许程序设置壁纸</td></tr><tr><td>android.permission.SET_WALLPAPER_HINTS</td><td>允许程序设置壁纸hits</td></tr><tr><td>android.permission.SIGNAL_PERSISTENT_PROCESSES</td><td>允许程序请求发送信号到所有显示的进程中</td></tr><tr><td>android.permission.STATUS_BAR</td><td>允许程序打开、关闭或禁用状态栏及图标</td></tr><tr><td>android.permission.SUBSCRIBED_FEEDS_READ</td><td>允许一个程序访问订阅RSS&#160;Feed内容提供</td></tr><tr><td>android.permission.SUBSCRIBED_FEEDS_WRITE</td><td>写入或修改订阅内容的数据库</td></tr><tr><td>android.permission.SYSTEM_ALERT_WINDOW</td><td>显示系统窗口</td></tr><tr><td>android.permission.UPDATE_DEVICE_STATS</td><td>更新设备状态</td></tr><tr><td>android.permission.USE_CREDENTIALS</td><td>允许程序请求验证从AccountManager</td></tr><tr><td>android.permission.USE_SIP</td><td>允许程序使用SIP视频服务</td></tr><tr><td>android.permission.VIBRATE</td><td>允许振动</td></tr><tr><td>android.permission.WAKE_LOCK</td><td>允许程序在手机屏幕关闭后后台进程仍然运行</td></tr><tr><td>android.permission.WRITE_APN_SETTINGS</td><td>写入网络GPRS接入点设置</td></tr><tr><td>android.permission.WRITE_CALENDAR</td><td>写入日程，但不可读取</td></tr><tr><td>android.permission.WRITE_CONTACTS</td><td>写入联系人，但不可读取</td></tr><tr><td>android.permission.WRITE_EXTERNAL_STORAGE</td><td>允许程序写入外部存储，如SD卡上写文件</td></tr><tr><td>android.permission.WRITE_GSERVICES</td><td>允许程序写入Google&#160;Map服务数据</td></tr><tr><td>com.android.browser.permission.WRITE_HISTORY_BOOKMARKS</td><td>写入浏览器历史记录或收藏夹，但不可读取</td></tr><tr><td>android.permission.WRITE_SECURE_SETTINGS</td><td>允许程序读写系统安全敏感的设置项</td></tr><tr><td>android.permission.WRITE_OWNER_DATA</td><td>允许一个程序写入但不读取所有者数据</td></tr><tr><td>android.permission.WRITE_SETTINGS</td><td>允许程序读取或写入系统设置</td></tr><tr><td>android.permission.WRITE_SMS</td><td>允许程序写短信</td></tr><tr><td>android.permission.WRITE_SYNC_SETTINGS</td><td>允许程序写入同步设置</td></tr><tr><td></td></tr></table>