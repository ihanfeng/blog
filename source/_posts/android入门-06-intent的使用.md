title: Android入门:-06-Intent的使用
date: 2013-12-26 16:08:35
categories: Android入门
tags: [Android入门,Intent]
---
##**什么是Intent**

Intent的中文意思是“意图，目的”的意思，可以理解为不同组件之间通信的“媒介”或者“信使”。

通过Intent，程序可以向android表达某种请求或者意愿，android会根据意愿的内容选择适当的组件来请求。

在组件之间的通讯，主要是有Intent协助完成的。Intent负责对应用中一次操作的动作，动作设计数据、附加数据进行描述吗，android则根据此Intent的描述，负责找到对应的组件，将Intent传递给调用的组件，并完成组件的调用。

<!--more-->
###**通用方式**

使用`intent.putExtra("name","hanfeng");`传递数据
使用`intent.getStringExtra("name")`获取数据


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

使用剪切板会用到，`ClipboardManager`对象，这个对用剪切板会用到，ClipboardManager象用来操作剪切板，但是没有提供public的构造函数（单例模式），需要使用`Activity.getSystemService(Context.CLIPBOARD_SERVICE)`获取该对象。

在Android-11（Android 3.0）版本之前，利用剪切板传递数据使用`setText()`和`getText()`方法，但是在此版本之后，这两个方法就被弃用，转而使用传递`ClipData`对象来代替。相对于getText和setText而言，利用ClipData对象来传递数据，更符合面向对象的思想，而且所能传递的数据类型也多样化了。

主要步骤：

1.通过`getSystemService`获取`ClipboardManager`对象cm。
2.使用`cm.setPrimaryClip()`方法设置ClipData数据对象。
3.在新Activity中获取`ClipboardManager`对象cm。
4.使用`cm.getPrimaryClip()`方法获取剪切板的`ClipData`数据对象，cd。
5.通过`cd.getItemAt(0)`获取到传递进来的数据。

下面通过具体代码来演示下：

保存数据到剪切板：
{%codeblock lang:java%}
package com.hanfeng.app;

import com.hanfeng.app.common.GlobalApp;

import android.os.Bundle;
import android.app.Activity;
import android.content.ClipData;
import android.content.ClipboardManager;
import android.content.Context;
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
	private Button button2 = null;

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

		button2 = (Button) this.findViewById(R.id.button2);
		button2.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {
				// 从android系统调用剪切板服务
				ClipboardManager clipboardManager = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
				clipboardManager.setPrimaryClip(ClipData.newPlainText("data", "hanfeng"));
				Intent intent = new Intent(MainActivity.this,
						OthertWOActivity.class);
				startActivity(intent);
			}
		});
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}

}

{%endcodeblock%}

接着创建一个新的Activity用来读取剪切板的数据

{%codeblock lang:java%}
package com.hanfeng.app;

import android.app.Activity;
import android.content.ClipData;
import android.content.ClipboardManager;
import android.content.Context;
import android.os.Bundle;
import android.widget.TextView;

public class OthertWOActivity extends Activity {
	
	private TextView textView =null;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		// TODO Auto-generated method stub
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_othertwo);
		
		ClipboardManager cm = (ClipboardManager)getSystemService(Context.CLIPBOARD_SERVICE);
		ClipData cd = cm.getPrimaryClip();
		
		String msg = cd.getItemAt(0).getText().toString();
		
		textView=(TextView)this.findViewById(R.id.textView1);
		textView.setText(msg);
	}
}
{%endcodeblock%}

这样就完成了一个简单的剪切板技术运用。

以上方式使用剪切板传递的为String类型的数据，如果需要传递一个对象，那么被传递的对象必须可序列化，序列化通过实现Serializable接口来标记。

主要步骤：

1.创建一个实现了Serializable接口的类SimpleData。
2.存入数据：获取ClipboardManager，并对通过Base64类对SimpleData对象进行序列化，再存入剪切板中。
3.取出数据：在新Activity中，获取ClipboardManager，对被序列化的数据进行反序列化，同样使用Base64类。然后对反序列化的数据进行处理。 

步骤一代码：
{%codeblock lang:java%}
package com.hanfeng.app.entity;

import java.io.Serializable;

public class SimpleData implements Serializable {

	private static final long serialVersionUID = 1L;

	private String name;
	private int age;

	public SimpleData(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

}
{%endcodeblock%}

第二部代码：
{%codeblock lang:java%}
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        Button btn=(Button)findViewById(R.id.btn);
        btn.setOnClickListener(new View.OnClickListener() {
            
            @SuppressLint("NewApi")
            @Override
            public void onClick(View v) {
                //获取剪切板
                ClipboardManager cm=(ClipboardManager)getSystemService(Context.CLIPBOARD_SERVICE);    
                
                MyData mydata=new MyData("jack", 24);
                String baseToString="";
                ByteArrayOutputStream bArr=new ByteArrayOutputStream();
                try
                {
                    ObjectOutputStream oos=new ObjectOutputStream(bArr);
                    oos.writeObject(mydata);
                    baseToString=Base64.encodeToString(bArr.toByteArray(), Base64.DEFAULT);
                    oos.close();
                }
                catch (Exception e)
                {
                    e.printStackTrace();
                    
                }
                
                cm.setPrimaryClip(ClipData.newPlainText("data",baseToString));
                Intent intent=new Intent(MainActivity.this,otherActivity.class);
                startActivity(intent);
            }
        });     
    }
{%endcodeblock%}

第三步代码：
{%codeblock lang:java%}
protected void onCreate(Bundle savedInstanceState) {
        // TODO Auto-generated method stub
        super.onCreate(savedInstanceState);
        setContentView(R.layout.other);
        
        ClipboardManager cm=(ClipboardManager)getSystemService(Context.CLIPBOARD_SERVICE);
        ClipData cd=cm.getPrimaryClip();
        
        String msg=cd.getItemAt(0).getText().toString();
        byte[] base64_btye=Base64.decode(msg, Base64.DEFAULT);
        ByteArrayInputStream bais=new ByteArrayInputStream(base64_btye);
        try {
            ObjectInputStream ois=new ObjectInputStream(bais);
            MyData mydata=(MyData)ois.readObject();
            
            TextView tv=(TextView)findViewById(R.id.msg);
            tv.setText(mydata.toString());
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }        
    }
{%endcodeblock%}

综上所述，使用剪切板传递数据有利有弊，剪切板为Android系统管理的，所以在一个地方存入的数据，在这个Android设备上任何应用都可以访问的到，但是正是因为此设备访问的都是同一个剪切板，可能会导致当前程序存入的数据，在使用前被其他程序覆盖掉了，导致无法保证正确获取数据。

###**使用静态变量传递数据**

原理其实很简单，就是在接收端的Avtivity里面设置static的变量，在发送端这边改变静态变量的值，然后启动意图。

直接看代码：MainActivity.java
{%codeblock lang:java%}
package com.intent.activity;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;

public class MainActivity extends Activity {
	private Button btn;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        btn = (Button)findViewById(R.id.btOpenOtherActivity);
        btn.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				//定义一个意图
				Intent intent = new Intent(MainActivity.this,OtherActivity.class);
				//改变OtherActivity的三个静态变量的值
				OtherActivity.name = "wulianghuan";
				OtherActivity.age = "22";
				OtherActivity.address = "上海闵行";
				startActivity(intent);
			}
		});
    }
}
{%endcodeblock%}


OtherActivity.java

{%codeblock lang:java%}
package com.intent.activity;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.TextView;

public class OtherActivity extends Activity {
	public static String name;
	public static String age;
	public static String address;
	private TextView text_name;
	private TextView text_age;
	private TextView text_address;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.other);
		text_name = (TextView) findViewById(R.id.name);
		text_age = (TextView) findViewById(R.id.age);
		text_address = (TextView) findViewById(R.id.address);	
		//设置文本框的数据
		text_name.setText("姓名："+name);
		text_age.setText("年龄："+age);
		text_address.setText("地址:"+address);
	}
}
{%endcodeblock%}

##**返回数据**

逻辑步骤：
1.`startActivityForResult(Intent intent,int i);`
启动下一个Activity要传送到下一个Activity的数据封装到intent中，并规定下一个Activity必须返回一个值i；
2.运行下一个Activity，并返回int i；
3.`onActivityResult(int requestCode,int ResultCode,Intent data)`
返回的i值此处传给requestCode,且只有当requestCode接受到的值是规定的i时此方法才能正确执行。

通俗理解： 
`startActivityForResult(Intent,int)`启动下一个Activity且规定下一个Activity应该返回一个值；
下一个Activity执行；
下一个Activity执行完，返回的值是规定的值时onActivityResult方法才能正确执行。

下面来一个例子：

主调函数通过下面的代码调用Activity和发送数据：
{%codeblock lang:java%}
 // 用Bundle来绑定所要的数据
Bundle bd = new Bundle();
bd.putDouble("height", height);
bd.putString("sex", sex);
             
// 创建一个新的Intent，并将Bundle传进去
Intent it = new Intent();
it.putExtras(bd);
it.setClass(ActivityDemo.this, Bundle2.class);
startActivityForResult(it, RESULT_OK);
主调函数通过onActivityResult回调传回数据：

    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
       // TODO Auto-generated method stub
       switch (resultCode) {
       case RESULT_OK:
           String sex = data.getStringExtra("sex");
           if(sex.equals("M")){
              rg.check(R.id.M);
           }else
              rg.check(R.id.F);
           break;
       default:
           break;
       }
    }
{%endcodeblock%}
被调函数通过下面代码返回数据：
{%codeblock lang:java%}
intent=this.getIntent();  
bunde = intent.getExtras();  
String sex = bundle.getString("sex");
Double height = bundle.getDouble("height");
Bundle2.this.setResult(RESULT_OK,intent);
Bundle2.this.finish();
{%endcodeblock%}