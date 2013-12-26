title: Android入门-04-Android生命周期
date: 2013-12-26 14:30:26
categories: Android入门
tags: [Android入门,生命周期]
---
##**什么是Activity**

Activity是Android的四大组件之一，可以用来显示View。Activity是一个与用户交互的系统模块，几乎所有的Activity都是和用户进行交互的。

首先我们来看下MVC的设计模式。

M(model 模型)：Model是应用程序的主体部分，所有的业务逻辑都应该写在这里面，如对数据库，对网络等操作都放在该层。

V(View 视图)：View是应用程序中负责生成用户界面的部分，也是整个MVC架构中用户唯一可以看到的一层，接收用户输入，显示出来结果。

C (Controller 控制层):android的控制层重任落在activity身上。
<!--more-->
##**Activity生命周期**

首先看一下Android api中所提供的Activity生命周期图：
![](/img/2014/01/android-activity-01.gif)

Activity其实是继承了ApplicationContext这个类，我们可以重写以下方法
{%codeblock lang:java%}
public class Activity extends ApplicationContext {
        protected void onCreate(Bundle savedInstanceState);
        
        protected void onStart();   
        
        protected void onRestart();
        
        protected void onResume();
        
        protected void onPause();
        
        protected void onStop();
        
        protected void onDestroy();
    }
{%endcodeblock%}

为了更好的理解生命周期，我们通过一个简单的Demo来坐下实验。

1.新建Android工程，命名为ActivityDemo

2.更改的生成的ActivityDemo.java，即重写上面的7个方法，使用Log进行打印，以便查看程序支持过程发生了什么。
{%codeblock lang:java%}
package com.hanfeng.app;

import android.os.Bundle;
import android.app.Activity;
import android.util.Log;
import android.view.Menu;

public class MainActivity extends Activity {

	private static final String TAG = "MainActivity";

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		Log.d(TAG, "start onCreate()");
	}

	@Override
	protected void onDestroy() {
		// TODO Auto-generated method stub
		super.onDestroy();
		Log.e(TAG, "start onDestroy()");  
	}

	@Override
	protected void onPause() {
		// TODO Auto-generated method stub
		super.onPause();
		Log.e(TAG, "start onPause()");  
	}

	@Override
	protected void onRestart() {
		// TODO Auto-generated method stub
		super.onRestart();
		Log.e(TAG, "start onRestart()");  
	}

	@Override
	protected void onResume() {
		// TODO Auto-generated method stub
		super.onResume();
		Log.e(TAG, "start onResume()");  
	}

	@Override
	protected void onStart() {
		// TODO Auto-generated method stub
		super.onStart();
		Log.e(TAG, "start onStart()");  
	}

	@Override
	protected void onStop() {
		// TODO Auto-generated method stub
		super.onStop();
		Log.e(TAG, "start onStop()");  
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}

}

{%endcodeblock%}

3.运行程序，查看Logcat视窗。（开始Activity）
![](/img/2014/01/android-activity-02.jpg)

从截图我们看到当我们打开应用的时候先后执行了onCreate()==>onStart()==>onResume()三个方法。

4.点击back键（关闭Activcity）

当我们按下BACK按钮时，这个程序将结束。这是调用OnPause()==>onStop()==>onDestory()三个方法。
如下图所示：
![](/img/2014/01/android-activity-03.jpg)

5.启动程序后点击HOME键 （重新获取Acticity焦点）
![](/img/2014/01/android-activity-04.jpg)
我们可以发现,当按下HOME键的时候，Activity执行了onPause()==>onStop()方法，应用程序并没有被销毁掉。

当我们再次启动ActivityDemo应用程序时候，先后执行了onRestart()==>onStart()==>onResume()三个方法。如下图：
![](/img/2014/01/android-activity-05.jpg)




