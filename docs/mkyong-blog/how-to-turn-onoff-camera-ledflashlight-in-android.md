# 如何在 Android 中打开/关闭相机 LED /闪光灯

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/how-to-turn-onoff-camera-ledflashlight-in-android/>

在本教程中，我们将向您展示如何在 Android 中打开/关闭手机摄像头 led 或手电筒。查看代码片段:

**1。打开**

```java
 camera = Camera.open();
	Parameters p = camera.getParameters();
	p.setFlashMode(Parameters.FLASH_MODE_TORCH);
	camera.setParameters(p);
	camera.startPreview(); 
```

**2。关闭**

```java
 camera = Camera.open();
	Parameters p = camera.getParameters();
	p.setFlashMode(Parameters.FLASH_MODE_OFF);
	camera.setParameters(p);
	camera.stopPreview(); 
```

并且，在`AndroidManifest.xml`上设置以下权限。

```java
 <uses-permission android:name="android.permission.CAMERA" />
    <uses-feature android:name="android.hardware.camera" /> 
```

*P.S 这个项目是在 Eclipse 3.7 中开发的，用**三星 Galaxy S2** (Android 2.3.3)测试过。*

## 1.Android 布局

只有一个按钮。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/relativeLayout1"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" >

    <Button
        android:id="@+id/buttonFlashlight"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
       	android:layout_centerVertical="true"
       	android:layout_centerHorizontal="true"
        android:text="Torch" />

</RelativeLayout> 
```

 ## 2.活动

看代码，一个开/关手电筒的按钮，应该不言自明。

```java
 package com.mkyong.android;

import android.app.Activity;
import android.content.Context;
import android.content.pm.PackageManager;
import android.hardware.Camera;
import android.hardware.Camera.Parameters;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;

public class FlashLightActivity extends Activity {

	//flag to detect flash is on or off
	private boolean isLighOn = false;

	private Camera camera;

	private Button button;

	@Override
	protected void onStop() {
		super.onStop();

		if (camera != null) {
			camera.release();
		}
	}

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		button = (Button) findViewById(R.id.buttonFlashlight);

		Context context = this;
		PackageManager pm = context.getPackageManager();

		// if device support camera?
		if (!pm.hasSystemFeature(PackageManager.FEATURE_CAMERA)) {
			Log.e("err", "Device has no camera!");
			return;
		}

		camera = Camera.open();
		final Parameters p = camera.getParameters();

		button.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View arg0) {

				if (isLighOn) {

					Log.i("info", "torch is turn off!");

					p.setFlashMode(Parameters.FLASH_MODE_OFF);
					camera.setParameters(p);
					camera.stopPreview();
					isLighOn = false;

				} else {

					Log.i("info", "torch is turn on!");

					p.setFlashMode(Parameters.FLASH_MODE_TORCH);

					camera.setParameters(p);
					camera.startPreview();
					isLighOn = true;

				}

			}
		});

	}
} 
```

 ## 3.Android 权限

分配**摄像机**权限。

*文件:AndroidManifest.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mkyong.android"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="10" />

    <uses-permission android:name="android.permission.CAMERA" />
    <uses-feature android:name="android.hardware.camera" />

    <application
        android:debuggable="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:label="@string/app_name"
            android:name=".FlashLightActivity" >
            <intent-filter >
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest> 
```

## 4.演示

没有，直到我有二手手机来捕捉我的手机上当前的手电筒:)

## 下载源代码

Download it – [Android-LED-FlashLight-Example.zip](http://web.archive.org/web/20190310101823/http://www.mkyong.com/wp-content/uploads/2012/03/Android-LED-FlashLight-Example.zip) (16 KB)

## 参考

1.  [安卓摄像头 Javadoc](http://web.archive.org/web/20190310101823/http://developer.android.com/reference/android/hardware/Camera.html)
2.  [Android 摄像头参数 Javadoc](http://web.archive.org/web/20190310101823/http://developer.android.com/reference/android/hardware/Camera.Parameters.html)

[android](http://web.archive.org/web/20190310101823/http://www.mkyong.com/tag/android/) [flashlight](http://web.archive.org/web/20190310101823/http://www.mkyong.com/tag/flashlight/) [led](http://web.archive.org/web/20190310101823/http://www.mkyong.com/tag/led/)







