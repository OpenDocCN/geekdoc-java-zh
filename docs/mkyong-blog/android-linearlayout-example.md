# Android 线性布局示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-linearlayout-example/>

在 Android 中， [LinearLayout](http://web.archive.org/web/20190223082342/http://developer.android.com/reference/android/widget/LinearLayout.html) 是一种常见的布局，通过`orientation`属性将“组件”按垂直或水平顺序排列。此外，最高的`weight`组件将填充`LinearLayout`中的剩余空间。

在本教程中，我们将向你展示如何使用`LinearLayout`以垂直和水平顺序显示 3 个按钮，以及“权重”是如何工作的。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

## 1.线性布局–水平

打开“ **res/layout/main.xml** ”文件，在`LinearLayout`内添加 3 个按钮，方向为“**水平**”。在这种情况下，权重最高的是“button3”，因此它将填满布局中的剩余空间。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="horizontal" >

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 2" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 3" 
        android:layout_weight="1"/>

</LinearLayout> 
```

见图:

![android linearlayout demo1](img/e3ff32f4c2f6ade31edcabfb54ec167e.png "android-linearlayout-horizontal") ## 2.线性布局–垂直

现在，将`LinearLayout`改为**垂直**方向。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 2" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 3" 
        android:layout_weight="1"/>

</LinearLayout> 
```

见图:

![android linearlayout demo2](img/f79c128bc7483893f6c89e93182b29e6.png "android-linearlayout-vertical") ## 下载源代码

Download it – [Android-LinearLayout-Example.zip](http://web.archive.org/web/20190223082342/http://www.mkyong.com/wp-content/uploads/2011/12/Android-LinearLayout-Example.zip) (15 KB)

## 参考

1.  [Android LinearLayout 示例](http://web.archive.org/web/20190223082342/http://developer.android.com/resources/tutorials/views/hello-linearlayout.html)
2.  [Android linear layout JavaDoc](http://web.archive.org/web/20190223082342/http://developer.android.com/reference/android/widget/LinearLayout.html)

[android](http://web.archive.org/web/20190223082342/http://www.mkyong.com/tag/android/) [layout](http://web.archive.org/web/20190223082342/http://www.mkyong.com/tag/layout/) [linearlayout](http://web.archive.org/web/20190223082342/http://www.mkyong.com/tag/linearlayout/)







