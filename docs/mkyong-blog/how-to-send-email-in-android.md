# 如何在 Android 中发送电子邮件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/how-to-send-email-in-android/>

在 Android 中，可以使用`Intent.ACTION_SEND`调用现有的邮件客户端发送邮件。

请参见以下代码片段:

```java
 Intent email = new Intent(Intent.ACTION_SEND);
	email.putExtra(Intent.EXTRA_EMAIL, new String[]{"youremail@yahoo.com"});		  
	email.putExtra(Intent.EXTRA_SUBJECT, "subject");
	email.putExtra(Intent.EXTRA_TEXT, "message");
	email.setType("message/rfc822");
	startActivity(Intent.createChooser(email, "Choose an Email client :")); 
```

这个项目是在 Eclipse 3.7 中开发的，并用三星 Galaxy S2 (Android 2.3.3)进行了测试。

**Run & test on real device only.**
If you run this on emulator, you will hit error message : “**No application can perform this action**“. This code only work on real device.

## 1.Android 布局

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/linearLayout1"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <TextView
        android:id="@+id/textViewPhoneNo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="To : "
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <EditText
        android:id="@+id/editTextTo"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:inputType="textEmailAddress" >

        <requestFocus />

    </EditText>

    <TextView
        android:id="@+id/textViewSubject"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Subject : "
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <EditText
        android:id="@+id/editTextSubject"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
         >
    </EditText>

    <TextView
        android:id="@+id/textViewMessage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Message : "
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <EditText
        android:id="@+id/editTextMessage"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="top"
        android:inputType="textMultiLine"
        android:lines="5" />

    <Button
        android:id="@+id/buttonSend"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Send" />

</LinearLayout> 
```

## 2.活动

发送电子邮件的完整活动类。看了一下`onClick()`方法，应该不言自明。

```java
 package com.mkyong.android;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;

public class SendEmailActivity extends Activity {

	Button buttonSend;
	EditText textTo;
	EditText textSubject;
	EditText textMessage;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		buttonSend = (Button) findViewById(R.id.buttonSend);
		textTo = (EditText) findViewById(R.id.editTextTo);
		textSubject = (EditText) findViewById(R.id.editTextSubject);
		textMessage = (EditText) findViewById(R.id.editTextMessage);

		buttonSend.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {

			  String to = textTo.getText().toString();
			  String subject = textSubject.getText().toString();
			  String message = textMessage.getText().toString();

			  Intent email = new Intent(Intent.ACTION_SEND);
			  email.putExtra(Intent.EXTRA_EMAIL, new String[]{ to});
			  //email.putExtra(Intent.EXTRA_CC, new String[]{ to});
			  //email.putExtra(Intent.EXTRA_BCC, new String[]{to});
			  email.putExtra(Intent.EXTRA_SUBJECT, subject);
			  email.putExtra(Intent.EXTRA_TEXT, message);

			  //need this to prompts email client only
			  email.setType("message/rfc822");

			  startActivity(Intent.createChooser(email, "Choose an Email client :"));

			}
		});
	}
} 
```

## 3.演示

查看默认屏幕，填写详细信息，然后单击“发送”按钮。

![send email in android](img/5b19f41c21a6c16ed2a451605338840a.png "Android-Send-Email-Example-1")

它会提示您现有的电子邮件客户端进行选择。

![send email in android](img/1b747bdee29ccd1514f9491e478f6481.png "Android-Send-Email-Example-2")

在这种情况下，我选择了 **Gmail** ，所有之前填写的详细信息将自动填充到 **Gmail** 客户端。

![send email in android](img/95b5ba61057bdb8b25c0be1304f10fd5.png "Android-Send-Email-Example-3")**Note**
Android does not provide API to send Email directly, you have to call the existing Email client to send Email.

## 下载源代码

Download it – [Android-Send-Email-Example.zip](http://web.archive.org/web/20220705094910/http://www.mkyong.com/wp-content/uploads/2012/03/Android-Send-Email-Example.zip) (16 KB)

## 参考

1.  [安卓意向动作 _ 发送 Javadoc](http://web.archive.org/web/20220705094910/https://developer.android.com/reference/android/content/Intent.html#ACTION_SEND)

<input type="hidden" id="mkyong-current-postId" value="10712">