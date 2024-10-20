# Android WebView 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-webview-example/>

Android 的 [WebView](http://web.archive.org/web/20210507040152/https://developer.android.com/reference/android/webkit/WebView.html) 允许你打开自己的窗口来查看 URL 或自定义 html 标记页面。

在本教程中，您将创建两个页面，一个页面有一个按钮，当您单击它时，它将导航到另一个页面，并在 WebView 组件中显示 URL“*google.com*”。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

## 1.Android 布局文件

创建两个 Android 布局文件——“*RES/layout/main . XML*”和“ *res/layout/webview.xml* ”。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Button
        android:id="@+id/buttonUrl"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Go to http://www.google.com" />

</LinearLayout> 
```

*文件:RES/layout/main . XML–WebView 示例*

```java
 <?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webView1"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
/> 
```

## 2.活动

两个活动类，一个活动显示一个按钮，另一个活动显示带有预定义 URL 的`WebView`。

*文件:MainActivity.java*

```java
 package com.mkyong.android;

import android.app.Activity;
import android.content.Context;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;

public class MainActivity extends Activity {

	private Button button;

	public void onCreate(Bundle savedInstanceState) {
		final Context context = this;

		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		button = (Button) findViewById(R.id.buttonUrl);

		button.setOnClickListener(new OnClickListener() {

		  @Override
		  public void onClick(View arg0) {
		    Intent intent = new Intent(context, WebViewActivity.class);
		    startActivity(intent);
		  }

		});

	}

} 
```

*文件:WebViewActivity.java*

```java
 package com.mkyong.android;

import android.app.Activity;
import android.os.Bundle;
import android.webkit.WebView;

public class WebViewActivity extends Activity {

	private WebView webView;

	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.webview);

		webView = (WebView) findViewById(R.id.webView1);
		webView.getSettings().setJavaScriptEnabled(true);
		webView.loadUrl("http://www.google.com");

	}

} 
```

## 3.Android 清单

`WebView`所需的**上网权限**，添加到下面的`AndroidManifest.xml`中。

```java
 <uses-permission android:name="android.permission.INTERNET" /> 
```

*文件:Android manifest . XML*–参见完整示例。

```java
 <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mkyong.android"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="10" />

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:name=".WebViewActivity"
            android:theme="@android:style/Theme.NoTitleBar" />

        <activity
            android:label="@string/app_name"
            android:name=".MainActivity" >
            <intent-filter >
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest> 
```

## 4.演示

默认情况下，只显示一个按钮。

![android webview example](img/9ec3a442ce3709f846f931bca755301c.png "android-webview-example")

点击按钮，显示 WebView。

![android webview example](img/aeff49bd62f87f3ec902dff47218ce44.png "android-webview-example-result")

## 5.再次演示

`WebView`允许您通过`webView.loadData()`手动加载自定义 HTML 标记，参见修改后的版本:

```java
 package com.mkyong.android;

import android.app.Activity;
import android.os.Bundle;
import android.webkit.WebView;

public class WebViewActivity extends Activity {

	private WebView webView;

	public void onCreate(Bundle savedInstanceState) {
	   super.onCreate(savedInstanceState);
	   setContentView(R.layout.webview);

	   webView = (WebView) findViewById(R.id.webView1);
	   webView.getSettings().setJavaScriptEnabled(true);
	   //webView.loadUrl("http://www.google.com");

	   String customHtml = "<html><body><h1>Hello, WebView</h1></body></html>";
	   webView.loadData(customHtml, "text/html", "UTF-8");

	}

} 
```

现在，当点击按钮时，会显示一个定制的 html 页面。

![android webview example](img/14cdf0b4891012b4acd2d47796f1b616.png "android-webview-example-result-2")

## 下载源代码

Download it – [Android-WebView-Example.zip](http://web.archive.org/web/20210507040152/http://www.mkyong.com/wp-content/uploads/2012/02/Android-WebView-Example.zip) (16 KB)

## 参考

1.  [官方 Android webView 示例](http://web.archive.org/web/20210507040152/https://developer.android.com/resources/tutorials/views/hello-webview.html)
2.  [Android WebView Javadoc](http://web.archive.org/web/20210507040152/https://developer.android.com/reference/android/webkit/WebView.html)
3.  [切换安卓活动](http://web.archive.org/web/20210507040152/http://www.mkyong.com/android/android-activity-from-one-screen-to-another-screen/)

Tags : [android](http://web.archive.org/web/20210507040152/https://mkyong.com/tag/android/) [webview](http://web.archive.org/web/20210507040152/https://mkyong.com/tag/webview/)<input type="hidden" id="mkyong-current-postId" value="10579">