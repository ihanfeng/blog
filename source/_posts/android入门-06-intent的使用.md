title: Android入门:-06-Intent的使用
date: 2013-12-26 16:08:35
categories: Android入门
tags: [Android入门,Intent]
---
##**什么是Intent**

Intent的中文意思是“意图，目的”的意思，可以理解为不同组件之间通信的“媒介”或者“信使”。

###**全局变量传递**

在Activity之间数据传递可以使用全局对象。类似于JAVA WEB里面的Application作用域。除非是Android应用程序清除内存，否则全局对象将一直都可以访问。

1、新建共享的全局变量

新建一个共享变量GlobalApp,继承自Application。
{%codeblock lang:java%}
package com.hanfeng.app.common;

import android.app.Application;

/**
 * 继承application，设置全局变量
 * 
 * @author hanfeng
 * 
 */
public class GlobalApp extends Application {
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

}
{%endcodeblock%}

2、配置AndroidMainifest.xml 

在AndroidMainifest.xml中声明一下全局变量的类，这时Android就会建立一个全局可用的实例
在Application属性中设置 `android:name="com.hanfeng.app.common.GlobalApp"`


3、在MainActivity类中设置全局变量
{%codeblock lang:java%}
package com.hanfeng.app;

import com.hanfeng.app.common.GlobalApp;

import android.os.Bundle;
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.Menu;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;

public class MainActivity extends Activity {

	private static final String TAG = "MainActivity";

	private GlobalApp globalApp = null;
	private Button button = null;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		button = (Button) this.findViewById(R.id.button1);
		button.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {
				// 获得应用程序实例
				globalApp = (GlobalApp) getApplicationContext();
				// 设置全局变量
				globalApp.setName("hanfneg");
				// 启动Activity
				Intent intent = new Intent(MainActivity.this,
						OtherActivity.class);
				startActivity(intent);

			}
		});

		Log.d(TAG, "start onCreate()");
	}

	@Override
	protected void onDestroy() {
		// TODO Auto-generated method stub
		super.onDestroy();
		Log.d(TAG, "start onDestroy()");
	}

	@Override
	protected void onPause() {
		// TODO Auto-generated method stub
		super.onPause();
		Log.d(TAG, "start onPause()");
	}

	@Override
	protected void onRestart() {
		// TODO Auto-generated method stub
		super.onRestart();
		Log.d(TAG, "start onRestart()");
	}

	@Override
	protected void onResume() {
		// TODO Auto-generated method stub
		super.onResume();
		Log.d(TAG, "start onResume()");
	}

	@Override
	protected void onStart() {
		// TODO Auto-generated method stub
		super.onStart();
		Log.d(TAG, "start onStart()");
	}

	@Override
	protected void onStop() {
		// TODO Auto-generated method stub
		super.onStop();
		Log.d(TAG, "start onStop()");
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}

}

{%endcodeblock%}

3、新建一个Activity类，用来获取全局变量

{%codeblock lang:java%}
package com.hanfeng.app;

import com.hanfeng.app.common.GlobalApp;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class OtherActivity extends Activity {
	private GlobalApp globalApp = null;
	private TextView textView = null;
	@Override
	protected void onCreate(Bundle savedInstanceState) {

		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_other);
		//获取应用程序实例
		globalApp = (GlobalApp)getApplicationContext();
		
		textView = (TextView)this.findViewById(R.id.textView1);
		textView.setText(globalApp.getName()); //取值并设置
		
	}
}

{%endcodeblock%}

###**剪切板传递**

在Activity之间数据传递还可以使用剪切板技术。就是某个程序将一些数据复制到剪切板上，然后其他的任何程序都可以从剪切板中获取数据。

