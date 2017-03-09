####状态栏了解
New一个新项目,在android 4.3，6.0上运行后如下:


![4.3_test1.png](http://upload-images.jianshu.io/upload_images/909565-5422000bada79b1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![6.0_test1.png](http://upload-images.jianshu.io/upload_images/909565-cb4399a1c1089029.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从图片中的顶部可以看到区别：4.3和6.0的状态栏，完全不同，为什么呢？
>4.4以下系统的状态栏都是黑色为背景的。

>4.4以上系统支持状态栏颜色的定制

>其中6.0上面的导航栏（标题栏）是theme里面带的，可以设置为Theme.AppCompat.Light.NoActionBar这样actionBar就没有了！

####问题1: 想要一个如下的全透明的效果, 设备是android 6.0的：

![device-2017-03-08-154635.png](http://upload-images.jianshu.io/upload_images/909565-b8ed60a7337305f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**实现思路（5.0以上版本, 其他的不管）：去掉标题栏，然后设置状态栏透明，布局适应系统。**

**解决方法：**在res下创建文件夹values-v21, 新建styles文件，创建一个和values下面同名的theme, 代码如下。
(values-vxx文件夹是由系统会根据SDK的版本选择合适的Theme进行设置)

![QQ截图20170308155310.png](http://upload-images.jianshu.io/upload_images/909565-d5ba5f1651dada49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后: AndroidManifest.xml文件来调用
![theme.png](http://upload-images.jianshu.io/upload_images/909565-cbf707c1b33f5f42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**说明：**
*  4.4 开始提供了<item name="android:windowTranslucentStatus">来处理状态栏的透明度，但是适应它需要最低19的api来实现，所以用不同级别的values来区分，不然之间使用会报错。

*  5.0开始提供<item name="android:statusBarColor">使用需要最低21的api来实现, 它是来设置状态栏的颜色的，可以设置其他的哦

**以上是测试是5.0，6.0的设备：全透明状态栏的测试OK**


####问题2：app中导航栏颜色扩展到状态栏一样


**效果图:**
![device-2017-03-09-102303.png](http://upload-images.jianshu.io/upload_images/909565-77442db0c00efef0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**调用values-v21/styles.xml中AppTheme**

> 实现状态栏的透明

```
<resources>
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="android:windowTranslucentStatus">false</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <!--<!–Android 5.x开始需要把颜色设置透明，否则导航栏会呈现系统默认的浅灰色–>-->
        <!--<!– 可以修改状态栏的颜色 -->
        <item name="android:statusBarColor">@android:color/transparent</item>
    </style>
</resources>
```

**布局代码：**

>设置android:fitsSystemWindows=true,是布局适应系统，并设置一样的颜色

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:background="@color/color_31c27c"
              android:fitsSystemWindows="true">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:gravity="center"
        android:text="新页面"
        android:textSize="20sp"
        android:textColor="#234234"
        android:background="@color/color_31c27c"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#ffffff"/>

</LinearLayout>

```

注意点：
1: fitSystemWindows属性
简单描述： 
这个一个boolean值的内部属性，让view可以根据系统窗口(如status bar)来调整自己的布局，如果值为true,就会调整view的paingding属性来给system windows留出空间…. 
实际效果： 
当status bar为透明或半透明时(4.4以上),系统会设置view的paddingTop值为一个适合的值(status bar的高度)让view的内容不被上拉到状态栏，当在不占据status bar的情况下(4.4以下)会设置paddingTop值为0(因为没有占据status bar所以不用留出空间)。


2: 上面的根布局LinearLayout 中, 如果不设置android:fitsSystemWindows="true"就会出现布局文件占据状态栏的情况，

>该属性的作用在于，不会让系统导航栏和我们app的UI重叠，导致交互问题。

![909565-abd972bbcc63e33c.png](http://upload-images.jianshu.io/upload_images/909565-c32c2a221a216150.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


3:   上面的根布局LinearLayout 中, 如果不加入**和标题栏一样的颜色布局**,会出现状态栏变灰色的情况


![909565-a0c741129b624765.png](http://upload-images.jianshu.io/upload_images/909565-a9036f5804c2c7f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![QQ截图20170309104446.png](http://upload-images.jianshu.io/upload_images/909565-f35aa4f475cec6aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后demo的效果，测试了5.1和6.0的都可以，**点击进入，可以看到“新页面”的效果**

![device-2017-03-09-105302.png](http://upload-images.jianshu.io/upload_images/909565-fb44fbab098881d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###总结上面的原理：

>1: 设置状态栏透明 (通过android:windowTranslucentStatus等)


>2: 使view适应系统 (通过android:fitsSystemWindows="true"等)


**demo地址：https://github.com/George-Soros/AndroidStatusbar**

参考：http://www.jianshu.com/p/0acc12c29c1b
网上还有一种方式是使用toolbar的方式http://www.jianshu.com/p/34a8b40b9308
