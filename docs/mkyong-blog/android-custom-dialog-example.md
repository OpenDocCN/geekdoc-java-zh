# Android 自定义对话框示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-custom-dialog-example/>

在本教程中，我们将向您展示如何在 Android 中创建自定义对话框。请参见以下步骤:

1.  创建自定义对话框布局(XML 文件)。
2.  将布局附在`Dialog`上。
3.  显示`Dialog`。
4.  完成了。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

**Note**
You may also interest to read this [custom AlertDialog example](http://web.archive.org/web/20220629075036/http://www.mkyong.com/android/android-prompt-user-input-dialog-example/).

## 1 个 Android 布局文件

两个 XML 文件，一个用于主屏幕，一个用于自定义对话框。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Button
        android:id="@+id/buttonShowCustomDialog"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Show Custom Dialog" />

</LinearLayout> 
```

*文件:res/layout/custom.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" >

    <ImageView
        android:id="@+id/image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginRight="5dp" />

    <TextView
        android:id="@+id/text"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:textColor="#FFF" 
        android:layout_toRightOf="@+id/image"/>/>

     <Button
        android:id="@+id/dialogButtonOK"
        android:layout_width="100px"
        android:layout_height="wrap_content"
        android:text=" Ok "
        android:layout_marginTop="5dp"
        android:layout_marginRight="5dp"
        android:layout_below="@+id/image"
        />

</RelativeLayout> 
```

## 2.活动

阅读下一步的评论和演示，这应该是自我探索。

*文件:MainActivity.java*

```java
 package com.mkyong.android;

import android.app.Activity;
import android.app.Dialog;
import android.content.Context;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;

public class MainActivity extends Activity {

	final Context context = this;
	private Button button;

	public void onCreate(Bundle savedInstanceState) {

		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		button = (Button) findViewById(R.id.buttonShowCustomDialog);

		// add button listener
		button.setOnClickListener(new OnClickListener() {

		  @Override
		  public void onClick(View arg0) {

			// custom dialog
			final Dialog dialog = new Dialog(context);
			dialog.setContentView(R.layout.custom);
			dialog.setTitle("Title...");

			// set the custom dialog components - text, image and button
			TextView text = (TextView) dialog.findViewById(R.id.text);
			text.setText("Android custom dialog example!");
			ImageView image = (ImageView) dialog.findViewById(R.id.image);
			image.setImageResource(R.drawable.ic_launcher);

			Button dialogButton = (Button) dialog.findViewById(R.id.dialogButtonOK);
			// if button is clicked, close the custom dialog
			dialogButton.setOnClickListener(new OnClickListener() {
				@Override
				public void onClick(View v) {
					dialog.dismiss();
				}
			});

			dialog.show();
		  }
		});
	}
} 
```

## 3.演示

启动它，显示“`main.xml`”布局。

![android custom dialog example](img/dda0311a69bbb9222895f0d51760d392.png "android-custom-dialog-example")

点击按钮，显示自定义对话框"`custom.xml`"布局，如果点击"确定"按钮，对话框将被关闭。

![android custom dialog example](img/692c304863d053b32e5bae9a61ffcbcc.png "android-custom-dialog-example-1")

## 下载源代码

Download it – [Android-Custom-Dialog-Example.zip](http://web.archive.org/web/20220629075036/http://www.mkyong.com/wp-content/uploads/2012/03/Android-Custom-Dialog-Example.zip) (16 KB)

## 参考

1.  [安卓对话框 Javadoc](http://web.archive.org/web/20220629075036/https://developer.android.com/reference/android/app/Dialog.html)
2.  [安卓对话框示例](http://web.archive.org/web/20220629075036/https://developer.android.com/guide/topics/ui/dialogs.html)
3.  [安卓相对布局示例](http://web.archive.org/web/20220629075036/http://www.mkyong.com/android/android-relativelayout-example/)
4.  [Android 提示对话框(自定义 AlertDialog)示例](http://web.archive.org/web/20220629075036/http://www.mkyong.com/android/android-prompt-user-input-dialog-example/)

<input type="hidden" id="mkyong-current-postId" value="10637">