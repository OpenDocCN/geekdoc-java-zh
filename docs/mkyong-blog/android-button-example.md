# Android 按钮示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-button-example/>

在 Android 中，只需使用“ [android.widget.Button](http://web.archive.org/web/20190301142942/http://developer.android.com/reference/android/widget/Button.html) ”类显示一个普通按钮即可。

在本教程中，我们向你展示如何显示一个普通的按钮，添加一个点击监听器，当用户点击按钮时，在你的 Android 的互联网浏览器中打开一个 URL。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

**Note**
For more advance function, like image, please refer to this [ImageButton example](http://web.archive.org/web/20190301142942/http://www.mkyong.com/android/android-imagebutton-example/) and also this [ImageButton selector example](http://web.archive.org/web/20190301142942/http://www.mkyong.com/android/android-imagebutton-selector-example/).

## 1.添加按钮

打开" **res/layout/main.xml** 文件，添加一个按钮。

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
        android:text="Button - Go to mkyong.com" />

</LinearLayout> 
```

 ## 2.代码代码

给按钮附加一个点击监听器。

当用户点击它时，打开手机浏览器并显示网址:*http://www.mkyong.com*。

*文件:MyAndroidAppActivity.java*

```java
 package com.mkyong.android;

import android.app.Activity;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.widget.Button;
import android.view.View;
import android.view.View.OnClickListener;

public class MyAndroidAppActivity extends Activity {

	Button button;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		addListenerOnButton();

	}

	public void addListenerOnButton() {

		button = (Button) findViewById(R.id.button1);

		button.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View arg0) {

			  Intent browserIntent = 
                            new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.mkyong.com"));
			    startActivity(browserIntent);

			}

		});

	}

} 
```

 ## 3.演示

运行应用程序。

1.结果，一个正常的按钮。

![android button demo1](img/d44ea3d5a3ad1681a87a6254d834d317.png "android-button-demo1")

2.点击按钮，在浏览器中显示网址。

![android button demo2](img/8adee7f10d56ceb3e96727ea80a2d0fd.png "android-button-demo2")

## 下载源代码

Download it – [Android-Button-Example.zip](http://web.archive.org/web/20190301142942/http://www.mkyong.com/wp-content/uploads/2011/12/Android-Button-Example.zip) (15 KB)

## 参考

1.  [安卓按钮 JavaDoc](http://web.archive.org/web/20190301142942/http://developer.android.com/reference/android/widget/Button.html)

[android](http://web.archive.org/web/20190301142942/http://www.mkyong.com/tag/android/) [button](http://web.archive.org/web/20190301142942/http://www.mkyong.com/tag/button/)







