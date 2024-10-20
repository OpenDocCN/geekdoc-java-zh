# Android 复选框示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-checkbox-example/>

在 Android 中，可以使用“ [android.widget.CheckBox](http://web.archive.org/web/20190306165505/http://developer.android.com/reference/android/widget/CheckBox.html) ”类来渲染一个复选框。

在本教程中，我们将向您展示如何在 XML 文件中创建 3 个复选框，并演示如何使用监听器来检查复选框的状态-选中或未选中。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

## 1.自定义字符串

打开" **res/values/strings.xml** 文件，添加一些用户自定义的字符串。

*文件:res/values/strings.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello World, MyAndroidAppActivity!
    <string name="app_name">MyAndroidApp
    <string name="chk_ios">IPhone
    <string name="chk_android">Android
    <string name="chk_windows">Windows Mobile
    <string name="btn_display">Display
</resources> 
```

 ## 2.检验盒

打开“ **res/layout/main.xml** 文件，在`LinearLayout`里面添加 3 个“**复选框**和一个按钮。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <CheckBox
        android:id="@+id/chkIos"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/chk_ios" />

    <CheckBox
        android:id="@+id/chkAndroid"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/chk_android"
        android:checked="true" />

    <CheckBox
        android:id="@+id/chkWindows"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/chk_windows" />

    <Button
        android:id="@+id/btnDisplay"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/btn_display" />

</LinearLayout> 
```

**Make CheckBox is checked by default**
Put `android:checked="true"` inside checkbox element to make it checked bu default. In this case, “Android” option is checked by default. ## 3.代码代码

在 activity " `onCreate()`"方法中附加侦听器，以监视以下事件:

1.  如果复选框 id : " **chkIos** "被选中，则显示一个浮动框，显示消息"兄弟，试试 Android "。
2.  如果按钮被单击，则显示一个浮动框并显示复选框状态。

*文件:MyAndroidAppActivity.java*

```java
 package com.mkyong.android;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Toast;

public class MyAndroidAppActivity extends Activity {

  private CheckBox chkIos, chkAndroid, chkWindows;
  private Button btnDisplay;

  @Override
  public void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.main);

	addListenerOnChkIos();
	addListenerOnButton();
  }

  public void addListenerOnChkIos() {

	chkIos = (CheckBox) findViewById(R.id.chkIos);

	chkIos.setOnClickListener(new OnClickListener() {

	  @Override
	  public void onClick(View v) {
                //is chkIos checked?
		if (((CheckBox) v).isChecked()) {
			Toast.makeText(MyAndroidAppActivity.this,
		 	   "Bro, try Android :)", Toast.LENGTH_LONG).show();
		}

	  }
	});

  }

  public void addListenerOnButton() {

	chkIos = (CheckBox) findViewById(R.id.chkIos);
	chkAndroid = (CheckBox) findViewById(R.id.chkAndroid);
	chkWindows = (CheckBox) findViewById(R.id.chkWindows);
	btnDisplay = (Button) findViewById(R.id.btnDisplay);

	btnDisplay.setOnClickListener(new OnClickListener() {

          //Run when button is clicked
	  @Override
	  public void onClick(View v) {

		StringBuffer result = new StringBuffer();
		result.append("IPhone check : ").append(chkIos.isChecked());
		result.append("\nAndroid check : ").append(chkAndroid.isChecked());
		result.append("\nWindows Mobile check :").append(chkWindows.isChecked());

		Toast.makeText(MyAndroidAppActivity.this, result.toString(),
				Toast.LENGTH_LONG).show();

	  }
	});

  }
} 
```

## 4.演示

运行应用程序。

1.结果:

![android checkbox demo 1](img/770bf47c609f2e05bae71b1cd664dff8.png "android-checkbox-demo1")

2.如果选中了“IPhone ”:

![android checkbox demo2 ](img/957b72b89f58deb56ee68552c8336933.png "android-checkbox-demo2")

3.勾选了“IPhone”和“Windows Mobile”，之后，点击“显示”按钮:

![android checkbox demo3](img/074d50d24297c31440d3b3d3f723cfd4.png "android-checkbox-demo3")

## 下载源代码

Download it – [Android-Checkbox-Example.zip](http://web.archive.org/web/20190306165505/http://www.mkyong.com/wp-content/uploads/2011/11/Android-Checkbox-Example.zip) (15 KB)

## 参考

1.  [Android 复选框 JavaDoc](http://web.archive.org/web/20190306165505/http://developer.android.com/reference/android/widget/CheckBox.html)
2.  [Android 复选框示例](http://web.archive.org/web/20190306165505/http://developer.android.com/resources/tutorials/views/hello-formstuff.html#Checkbox)

[android](http://web.archive.org/web/20190306165505/http://www.mkyong.com/tag/android/) [checkbox](http://web.archive.org/web/20190306165505/http://www.mkyong.com/tag/checkbox/)







