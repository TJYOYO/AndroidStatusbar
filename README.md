# AndroidStatusbar

####状态栏了解
New一个新项目,在android 4.3，6.0上运行后如下:


![4.3_test1.png](http://upload-images.jianshu.io/upload_images/909565-5422000bada79b1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![6.0_test1.png](http://upload-images.jianshu.io/upload_images/909565-cb4399a1c1089029.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从图片中的顶部可以看到区别：4.3和6.0的状态栏，完全不同，为什么呢？
>4.4以下系统的状态栏都是黑色为背景的。

>4.4以上系统支持状态栏颜色的定制

>5.0上是完全透明，6.0上又是半透明

>其中6.0上面的导航栏（标题栏）是theme里面带的，可以设置为Theme.AppCompat.Light.NoActionBar这样actionBar就没有了！

####问题1: 想要一个如下的全透明的效果, 设备是android 6.0的：

![device-2017-03-08-154635.png](http://upload-images.jianshu.io/upload_images/909565-b8ed60a7337305f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**实现思路（5.0以上版本, 其他的不管）：**去掉标题栏，然后设置状态栏透明，布局适应系统。

**解决方法：**在res下创建文件夹values-v21, 新建styles文件，创建一个和values下面同名的theme, 代码如下。
(values-vxx文件夹是由系统会根据SDK的版本选择合适的Theme进行设置)

![QQ截图20170308155310.png](http://upload-images.jianshu.io/upload_images/909565-d5ba5f1651dada49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后: AndroidManifest.xml文件来调用
![theme.png](http://upload-images.jianshu.io/upload_images/909565-cbf707c1b33f5f42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**说明：**
*  4.4 开始提供了<item name="android:windowTranslucentStatus">来处理状态栏的透明度，但是适应它需要最低19的api来实现，所以用不同级别的values来区分，不然之间使用会报错。

*  5.0开始提供<item name="android:statusBarColor">使用需要最低21的api来实现, 它是来设置状态栏的颜色的，可以设置其他的哦

**以上是测试是5.0，6.0的设备：全透明状态栏的测试OK**


####问题2：app中导航栏颜色为纯色的界面


**效果图:**
![device-2017-03-09-102303.png](http://upload-images.jianshu.io/upload_images/909565-77442db0c00efef0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**调用values-v21/styles.xml中AppTheme**
```
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
```

**布局代码：**
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

1: 上面的根布局LinearLayout 中, 如果不设置android:fitsSystemWindows="true"就会出现布局文件占据状态栏的情况，设置了该属性的作用在于，不会让系统导航栏和我们app的UI重叠，导致交互问题。

![device-2017-03-09-103019.png](http://upload-images.jianshu.io/upload_images/909565-abd972bbcc63e33c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2:   上面的根布局LinearLayout 中, 如果不加入**和标题栏一样的颜色布局**,会出现状态栏变灰色的情况

![device-2017-03-09-104557.png](http://upload-images.jianshu.io/upload_images/909565-a0c741129b624765.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![QQ截图20170309104446.png](http://upload-images.jianshu.io/upload_images/909565-f35aa4f475cec6aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后demo的效果，**点击进入，可以看到“新页面”的效果**

![device-2017-03-09-105302.png](http://upload-images.jianshu.io/upload_images/909565-fb44fbab098881d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**demo地址：**



参考：http://www.jianshu.com/p/0acc12c29c1b
