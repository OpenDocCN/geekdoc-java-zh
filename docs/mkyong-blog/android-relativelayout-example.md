# Android RelativeLayout 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-relativelayout-example/>

在 Android 中， [RelativeLayout](http://web.archive.org/web/20190309055103/http://developer.android.com/reference/android/widget/RelativeLayout.html) 允许你根据附近(相对或兄弟)组件的位置来定位你的组件。这是最灵活的布局，允许你将你的组件放置在任何你想显示的地方(如果你知道如何“相对”它)。

在`RelativeLayout`中，可以使用上方的**、**下方的**、**左侧的**和**右侧的**来排列组件的位置，例如在“按钮 2”下方显示一个“按钮 1”，或者在“按钮 1”的右侧显示“按钮 3”。**

**Note**
The `RelativeLayout` is very flexible, but hard to master it. Suggest you use Eclipse IDE to drag the component, then view study the Eclipse generated XML layout code to understand how to code “relative” components.

在本教程中，我们将向您展示如何通过`RelativeLayout`来排列/定位`button`、`textview`和`editbox`。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

## 1.相对布局

打开“ **res/layout/main.xml** 文件，添加组件，通过“`RelativeLayout`定位。阅读下面的 XML 代码，非常详细地告诉你在哪里显示组件。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" >

    <Button
        android:id="@+id/btnButton1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1"/>

    <Button
        android:id="@+id/btnButton2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 2"
        android:layout_toRightOf="@+id/btnButton1"/>

     <Button
        android:id="@+id/btnButton3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 3"
        android:layout_below="@+id/btnButton1"/>

     <TextView
         android:id="@+id/textView1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:layout_below="@+id/btnButton3"
         android:layout_marginTop="94dp"
         android:text="User :"
         android:textAppearance="?android:attr/textAppearanceLarge" />

     <EditText
         android:id="@+id/editText1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:layout_alignParentRight="true"
         android:layout_alignTop="@+id/textView1"
         android:layout_toRightOf="@+id/btnButton3" />

     <Button
         android:id="@+id/btnSubmit"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:layout_alignParentRight="true"
         android:layout_below="@+id/editText1"
         android:text="Submit" />

</RelativeLayout> 
```

 ## 2.演示

请参见结果，上面的 XML 代码将生成以下输出。

![android relativelayout demo](img/88b22dd990af248ae6e57dcd1bf2eb97.png "android-relative-layout-demo") ## 下载源代码

Download it – [Android-RelativeLayout-Example.zip](http://web.archive.org/web/20190309055103/http://www.mkyong.com/wp-content/uploads/2011/12/Android-RelativeLayout-Example.zip) (15 KB)

## 参考

1.  [Android RelativeLayout 示例](http://web.archive.org/web/20190309055103/http://developer.android.com/resources/tutorials/views/hello-relativelayout.html)
2.  [Android relative layout JavaDoc](http://web.archive.org/web/20190309055103/http://developer.android.com/reference/android/widget/RelativeLayout.html)

[android](http://web.archive.org/web/20190309055103/http://www.mkyong.com/tag/android/) [layout](http://web.archive.org/web/20190309055103/http://www.mkyong.com/tag/layout/) [relativelayout](http://web.archive.org/web/20190309055103/http://www.mkyong.com/tag/relativelayout/)







