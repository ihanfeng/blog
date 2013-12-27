title: Android基础-01-UI介绍编程
date: 2013-12-27 11:45:53
categories: Android基础
tags: [Android基础,UI]
---
##**视图组件与容器组件**

UI组件放在`android.widget`包及其子包、`android.View`包及其子包中，Android应用的所有UI组件都继承了View类。

View类重要子类：ViewGroup，通常作为其他组件的容器使用。

可以通过阅读官方APi文档来进行android的学习和开发。

推荐使用XML布局文件的方式来进行最近的开发。其本质和java代码是一样的。
<!--more-->
View类的XML属性、相关方法及说明

![](/img/2014/01/android-ui-01.jpg)
![](/img/2014/01/android-ui-02.jpg)
![](/img/2014/01/android-ui-03.jpg)
![](/img/2014/01/android-ui-04.jpg)
![](/img/2014/01/android-ui-05.jpg)
![](/img/2014/01/android-ui-06.jpg)
![](/img/2014/01/android-ui-07.jpg)


ViewGroup继承了View类，当然也可以当成普通的View来使用，但ViewGroup主要还是当成容器类使用。由于ViewGroup是一个抽象类，在实际使用中是使用ViewGroup的子类来作为容器。

ViewGroup容器控制其子组件的分布依赖于ViewGroup.layoutParams、ViewGroup、MarginLayoutParams两个内部类。

ViewGroup子元素支持的属性：

- android:layout_height：指定子组件的布局高度
- android:layout_width：指定子组件的布局宽度

属性值：

- fill_parent：指定子组件的高度、宽度与父容器组件的高度、宽度相同
- match_parent：与fill_parent相同。
- wrap+content：指定子组件的大小恰好能包裹它的内容即可。

答疑：布局高度与布局宽度和组件高度与组件宽度？

Androi布局机制决定的，android组件的大小不仅受它实际的高度、宽度控制，还能受它的布局高度与布局宽度控制。如设置组件宽度为30px，如果将布局宽度设置为match_parent，那么组件的宽度就会被拉宽到沾满它所在的父容器，如果将它的布局高度设为wrap_content，那么该组件的宽度才会是30px。

MarginLayoutParams用来控制子组件周围的页边距(Margin，也就是组件四周的空白)。
![](/img/2014/01/android-ui-08.jpg)

###**使用XML布局文件布局**

在res/layout目录下定义一个主文件名为任意的XML布局。通过android：id属性知道UI的唯一标识，然后在java代码中通过findViewById(R.id.<android.id属性>).

程序获取知道UI组件后，就可以通过代码控制各UI组件的外观行为，包括为UI组件绑定事件监听器等。

###**使用JAVA代码布局文件**
太繁琐，不利于解耦。不推荐使用。

###**XML和JAVA代码混合布局**

习惯把变化小，行为比较固定的组件放在XML布局文件中管理，而那些变化较多、行为控制较为复杂的组件则交给JAVA代码来管理。

###**开发自定义View**

基于UI组件的实现原理，开发者完全可以自己定义组件，通过继承View来派生自定义组件，然后重写View类的一个或多个方法。
     
构造器：重写构造器是定制View的最基本方式，当java代码创建一个View实例，或者根据XML布局文件加载并构建界面时将需要调用该构造器。

- onFinishInflate()：这是一个回调方法，当应用从XML布局文件加载该组件并利用它来构建界面后，该方法将会被回调。
- onMeasure(int,int)：调用该方法来检测View组件以及所包含的所有子组件的大
![](/img/2014/01/android-ui-09.jpg)

###**自定义View实例：跟随手指的小球**

自定义UI组件在指定位置绘制一个小球，这个位置可以动态改变，当用户通过手指在屏幕上拖动时，程序监听到这个动作，并把手指动作的位置传入自定义UI组件，并通知该组件重绘。

自定义组件代码：
{%codeblock lang:java%}
package com.hanfeng.app.view;
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.view.MotionEvent;
import android.view.View;
public class DrawView extends View {
    public float currentX = 40;
    public float currentY = 50;
    //创建并定义画笔
    Paint p = new Paint();
    public DrawView(Context context) {
        super(context);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        // TODO Auto-generated method stub
        super.onDraw(canvas);
        //设置画笔颜色
        p.setColor(Color.RED);
        //绘制小圆
        canvas.drawCircle(currentX, currentY, 15, p);
    }
    //为组件的触屏事件重写
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        // TODO Auto-generated method stub
        currentX = event.getX();
        currentY = event.getY();
        invalidate();
        return true;
    }
}
{%endcodeblock%}

上述DrawView组件继承View基类，并重写onDraw方法-该方法负责在该组件的指定位置绘制一个小球。还重写onTouchEvent(MotionEvent event)方法，该方法用于处理该组件的碰触事件，当用户手指碰触该组件就会激发该方法。

定义组件后通过JAVA代码把该组件添加到指定容器。

{%codeblock lang:java%}
//获取布局文件中LinearLayout容器
LinearLayout main = (LinearLayout)findViewById(R.id.root);
//创建DrawView组件
final DrawView draw = new DrawView(this);
//设置组件的最大宽度和高度
draw.setMinimumHeight(500);
draw.setMinimumWidth(300);
main.addView(draw);
{%endcodeblock%}

运行即可获得结果。
