# 如何为 Android 应用程序设置默认活动

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/how-to-set-default-activity-for-android-application/>

在 Android 中，您可以通过“ **AndroidManifest.xml** ”中的“ **intent-filter** ”来配置应用程序的启动活动(默认活动)。

请参见以下代码片段，将活动类" **logoActivity** "配置为默认活动。

文件:AndroidManifest.xml

```java
 <activity
            android:label="Logo"
            android:name=".logoActivity" >
             <intent-filter >
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity> 
```

例如，假设您有两个 activity 类，并且您想要将"`ListMobileActivity`"活动设置为您的应用程序的起始活动。

*文件:AndroidManifest.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mkyong.android"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="10" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:label="List of Mobile OS"
            android:name=".ListMobileActivity" >
            <intent-filter >
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:label="List of Fruits"
            android:name=".ListFruitActivity" >
        </activity>
    </application>

</manifest> 
```

另一方面，如果您想将“`ListFruitActivity`”活动设置为您的开始活动，只需剪切并粘贴“**”意向过滤器**，如下所示:

*文件:AndroidManifest.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mkyong.android"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="10" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:label="List of Mobile OS"
            android:name=".ListMobileActivity" >
        </activity>
        <activity
            android:label="List of Fruits"
            android:name=".ListFruitActivity" >
             <intent-filter >
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest> 
```

Tags : [android](http://web.archive.org/web/20210204055136/https://mkyong.com/tag/android/) [android activity](http://web.archive.org/web/20210204055136/https://mkyong.com/tag/android-activity/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="10444">