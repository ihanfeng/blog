title: android基础-02-ui布局管理器
date: 2013-12-27 16:07:07
categories: Android基础 
tags: [Android基础,UI布局管理器]
---
本篇主要介绍以ViewGroup为基类派生的布局管理器。

布局管理器可以根据运行平台来调整组件的大小，程序员只要为容器选择合适的布局管理器即可。Android的布局管理器本身就是一个UI组件，所有的布局管理器都是`ViewGroup`的子类。
<!--more-->
##**线性布局（LinearLayout**

将容器里的组件一个挨着一个地排列起来。`LinearLayout`可以控制各组件的横向排列。`android:orientation`属性进行控制，也可以控制组件纵向排列，该布局不会换行，当组件一个挨着一个地排列到头后，剩下的组件不会被显示出来。

常用属性及方法

- **android:baselineAligned：**属性设置为false，将会阻止该布局管理器与它的子元素基线对齐。
- **android:divider**：设置垂直布局时两个按钮之间的分隔条
- **android:gravity**：设置布局管理器内组件的对齐方式。支持各种属性top、buttom、left、right、center_vertical等
- **android:measureWithargestChild：**属性为ture，所有带权重的子元素都会具有最大子元素的最小尺寸
- **android:orientation:**设置布局管理器内组件的排列方式，horizontal(水平排列)、vertical（垂直排列、默认值）

LinearLayout包含的所有子元素都受LinearLayout.LayoutParams控制。

- **android:layout_gravity：**指定该子元素在LinearLayout中的对齐方式
- **android:layout_weight：**指定该子元素在LinearLayout中所占的权重

##**表格布局（TableLayout）**
继承LinerarLayout，本质是线性布局。表格布局采用行、列的形式来管理UI组件。通过TableRow、其他组件来控制表格的行数和列数。添加一个TableRow，该TableRow就是一个表格行，TableRow是容器，因此也可以不断往里头添加其他组件，每添加一个子组件该表格就增加一列。

列宽度由该列中最宽的单元格决定，整个表格布局宽度则取决于父容器的宽度。

单元格设置方式：

- **Shrinkable：**如果某个列被设为Shrinkable，那么该列的所有单元格的宽度都可被收缩，以保证该表格能适应父容器的宽度。
- **Stretchable：**该别的所有单元格的宽度都可以笨哦拉伸，以保证组件能完全填满表格空余空间。
- **Collapsed：**该列的所有单元格都会被隐藏

支持的属性：

- **android:collapseColumns：**设置需要被隐藏的列的序列号
- **android:shrinkColumns：**设置允许被收缩的列的序列号
- **android:stretchColumns：**设置允许被拉伸的列的序列号

代码如下：
{%codeblock lang:xml%}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" 
    android:orientation="vertical"
    >
    
    <!-- 定义第一个表格布局 第二列运行收缩 第三列运行拉伸 -->
    <TableLayout android:id="@+id/TableLayout01"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:shrinkColumns="1"
        android:stretchColumns="2"
        >
        <Button android:id="@+id/ok1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="独自一行的按钮"
            />
        <TableRow>
            <Button android:id="@+id/ok2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="普通按钮"
            />
            <Button android:id="@+id/ok3"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="收缩按钮"
            />
            <Button android:id="@+id/ok4"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="拉伸按钮"
            />
            
        </TableRow>
    </TableLayout>
    
    <!-- 定义第二个表格布局  指定第2列隐藏 -->
    <TableLayout 
        android:id="@+id/TableLayout02"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:collapseColumns="1"
        >
         <Button android:id="@+id/ok5"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="独自一行的按钮"
            />
          <TableRow>
            <Button android:id="@+id/ok6"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="普通按钮1"
            />
            <Button android:id="@+id/ok7"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="普通按钮2"
            />
            <Button android:id="@+id/ok8"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="普通按钮3"
            />
            
        </TableRow>
    </TableLayout>
    
    <!-- 定义第三个表格布局 第二、三列可以被拉伸 -->
    <TableLayout 
        android:id="@+id/TableLayout03"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:stretchColumns="1,2"
        >
         <Button android:id="@+id/ok9"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="独自一行的按钮"
         />
        <TableRow>
            <Button android:id="@+id/ok10"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="普通按钮1"
            />
            <Button android:id="@+id/ok11"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="拉伸按钮1"
            />
            <Button android:id="@+id/ok12"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="拉伸按钮2"
            />
            
        </TableRow>
    </TableLayout>
    
</LinearLayout>
{%endcodeblock%}

##**帧布局（FrameLayout）**
继承ViewGroup组件。该布局为每个加入其中的组件创建一个空白的区域，每个子组件占据一帧，这些帧会根据gravity属性执行自动对齐。

使用属性：

- **android:foreground：**设置该帧布局容器的前景图像
- **android:foregroundGravity：**定义绘制前景图像的gravity属性

##**相对布局（RelativeLayout）**
相对布局容器内子组件的位置总是相对兄弟组件、父容器来决定的。

属性：
- **android:grivaty：**设置布局容器内各子组件的对齐方式
- **android:ignoreGravity：**设置哪个组件不受gravity属性的影响。
为控制布局容器内的各子组件的布局分布，RelativeLayout提供一个内部类，RelativeLayout.LayoutParams。

![](/img/2014/01/android-ui-10.jpg)
![](/img/2014/01/android-ui-11.jpg)

##**网格布局（GridLayout）**
Android4.0 新增布局。类似HTML中的table标签，把整个容器划分成rows*columns个网格，每个网格可以放置一个组件。
通过setRowCount(int)和setColumnCount(int)方法来控制该网格的行数量和列数量。

![](/img/2014/01/android-ui-11.jpg)
![](/img/2014/01/android-ui-12.jpg)

##**绝对布局（AbsoluteLayout）**

已经不符合适宜，不在采用。
