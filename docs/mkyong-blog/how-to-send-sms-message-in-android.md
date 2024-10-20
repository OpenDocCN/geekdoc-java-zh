# 如何在安卓系统中发送短信

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/how-to-send-sms-message-in-android/>

在 Android 中，你可以使用`SmsManager` API 或者设备的`Built-in SMS`应用来发送短信。在本教程中，我们将向您展示两个发送短信的基本示例:

1.  SmsManager API

    ```java
     SmsManager smsManager = SmsManager.getDefault();
    	smsManager.sendTextMessage("phoneNo", null, "sms message", null, null); 
    ```

2.  内置短信应用

    ```java
     Intent sendIntent = new Intent(Intent.ACTION_VIEW);
    	sendIntent.putExtra("sms_body", "default content"); 
    	sendIntent.setType("vnd.android-dir/mms-sms");
    	startActivity(sendIntent); 
    ```

当然，两者都需要 **SEND_SMS** 权限。

```java
 <uses-permission android:name="android.permission.SEND_SMS" /> 
```

这个项目是在 Eclipse 3.7 中开发的，并用三星 Galaxy S2 (Android 2.3.3)进行了测试。

**Note**
The Built-in SMS application solution is the easiest way, because you let device handle everything for you.

## 1.SmsManager 示例

Android 布局文件的文本框(电话号码，短信)和按钮发送短信。

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
        android:text="Enter Phone Number : "
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <EditText
        android:id="@+id/editTextPhoneNo"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:phoneNumber="true" >
    </EditText>

    <TextView
        android:id="@+id/textViewSMS"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Enter SMS Message : "
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <EditText
        android:id="@+id/editTextSMS"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:inputType="textMultiLine"
        android:lines="5"
        android:gravity="top" />

    <Button
        android:id="@+id/buttonSend"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Send" />

</LinearLayout> 
```

*文件:SendSMSActivity.java*——通过`SmsManager`发送短信的活动。

```java
 package com.mkyong.android;

import android.app.Activity;
import android.os.Bundle;
import android.telephony.SmsManager;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class SendSMSActivity extends Activity {

	Button buttonSend;
	EditText textPhoneNo;
	EditText textSMS;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		buttonSend = (Button) findViewById(R.id.buttonSend);
		textPhoneNo = (EditText) findViewById(R.id.editTextPhoneNo);
		textSMS = (EditText) findViewById(R.id.editTextSMS);

		buttonSend.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {

			  String phoneNo = textPhoneNo.getText().toString();
			  String sms = textSMS.getText().toString();

			  try {
				SmsManager smsManager = SmsManager.getDefault();
				smsManager.sendTextMessage(phoneNo, null, sms, null, null);
				Toast.makeText(getApplicationContext(), "SMS Sent!",
							Toast.LENGTH_LONG).show();
			  } catch (Exception e) {
				Toast.makeText(getApplicationContext(),
					"SMS faild, please try again later!",
					Toast.LENGTH_LONG).show();
				e.printStackTrace();
			  }

			}
		});
	}
} 
```

*文件:AndroidManifest.xml* ，需要 **SEND_SMS** 权限。

```java
 <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mkyong.android"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="10" />

    <uses-permission android:name="android.permission.SEND_SMS" />

    <application
        android:debuggable="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:label="@string/app_name"
            android:name=".SendSMSActivity" >
            <intent-filter >
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest> 
```

参见演示:

![send sms message via smsmanager](img/1246663482afa05d7787e9d46b08eba9.png "android-send-sms-message-example") ## 2.内置短信应用示例

该示例使用设备的内置 SMS 应用程序发送 SMS 消息。

*File:RES/layout/main . XML*–仅一个按钮。

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/linearLayout1"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Button
        android:id="@+id/buttonSend"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Send" />

</LinearLayout> 
```

*文件:SendSMSActivity.java*–使用内置短信发送短信的活动类。

```java
 package com.mkyong.android;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.Toast;

public class SendSMSActivity extends Activity {

	Button buttonSend;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		buttonSend = (Button) findViewById(R.id.buttonSend);

		buttonSend.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {

				try {

				     Intent sendIntent = new Intent(Intent.ACTION_VIEW);
				     sendIntent.putExtra("sms_body", "default content"); 
				     sendIntent.setType("vnd.android-dir/mms-sms");
				     startActivity(sendIntent);

				} catch (Exception e) {
					Toast.makeText(getApplicationContext(),
						"SMS faild, please try again later!",
						Toast.LENGTH_LONG).show();
					e.printStackTrace();
				}
			}
		});
	}
} 
```

参见演示:

![send sms via build-in sms application](img/53e09212990ac13161fbff9de0d1e833.png "android-send-sms-message-example2")![send sms via build-in sms application](img/e522d4faab5ce3cc8ef6bda9b28f0163.png "android-send-sms-message-example2-1") ## 下载源代码

Download it – [1\. Android-Send-SMS-Example.zip](http://web.archive.org/web/20190309054809/http://www.mkyong.com/wp-content/uploads/2012/03/Android-Send-SMS-Example.zip) (16 KB)Download it – [2\. Android-Build-In-SMS-Application-Example.zip](http://web.archive.org/web/20190309054809/http://www.mkyong.com/wp-content/uploads/2012/03/Android-Build-In-SMS-Application-Example.zip) (16 KB)

## 参考

1.  [Android SmsManager Javadoc](http://web.archive.org/web/20190309054809/http://developer.android.com/reference/android/telephony/gsm/SmsManager.html)
2.  [Android 手机短信](http://web.archive.org/web/20190309054809/http://mobiforge.com/developing/story/sms-messaging-android)

[android](http://web.archive.org/web/20190309054809/http://www.mkyong.com/tag/android/) [sms](http://web.archive.org/web/20190309054809/http://www.mkyong.com/tag/sms/)







