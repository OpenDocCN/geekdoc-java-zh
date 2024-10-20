# Android——如何将按钮置于屏幕中央

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-how-to-center-button-on-screen/>

一个小的 Android 提示，告诉你如何在屏幕上居中按钮。将按钮包裹在`RelativeLayout`中，并将以下属性设置为“*真*”。

```java
 android:layout_centerVertical="true"
android:layout_centerHorizontal="true" 
```

下面是一个完整的例子，演示了如何使用上面的提示在屏幕上居中一个按钮。

## 1.Android 布局

布局中的按钮。

文件:res/layout/main.xml

```java
 <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/relativeLayout1"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" >

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
       	android:layout_centerVertical="true"
       	android:layout_centerHorizontal="true"
        android:text="Button" />

</RelativeLayout> 
```

 ## 2.活动

简单的活动类，什么也不做，只显示上面的布局文件。

```java
 package com.mkyong.android;

import android.app.Activity;
import android.os.Bundle;

public class HelloWorldActivity extends Activity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
    }
} 
```

 ## 3.演示

启动它，并查看输出:

![center button on screen](img/102e8c5dce1aab249d52cbe4545b7fa4.png "android-center-button-on-screen")

## 下载源代码

Download it – [Android-Center-Button-On-Screen-Example.zip](http://web.archive.org/web/20190223085505/http://www.mkyong.com/wp-content/uploads/2012/03/Android-Center-Button-On-Screen-Example.zip) (16 KB)

## 参考

1.  [Android relative layout layout params Javadoc](http://web.archive.org/web/20190223085505/http://developer.android.com/reference/android/widget/RelativeLayout.LayoutParams.html)

[android](http://web.archive.org/web/20190223085505/http://www.mkyong.com/tag/android/) [button](http://web.archive.org/web/20190223085505/http://www.mkyong.com/tag/button/) [center](http://web.archive.org/web/20190223085505/http://www.mkyong.com/tag/center/)







