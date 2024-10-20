# Android 进度条示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-progress-bar-example/>

在 Android 中，进度条可以告诉用户任务需要更长的时间来完成。

在本教程中，我们将向您展示如何显示一个进度条对话框来告诉用户您的任务正在运行，以及如何增加进度条状态直到任务完成。

**Note**
Refer to this [Android ProgressBar JavaDoc](http://web.archive.org/web/20190210022552/http://developer.android.com/reference/android/widget/ProgressBar.html) for detail explanation.

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

## 1.添加按钮

打开“ **res/layout/main.xml** 文件，只需添加普通按钮进行演示即可。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Button
        android:id="@+id/btnStartProgress"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Download File" />

</LinearLayout> 
```

 ## 2.代码代码

使用进度条的关键是使用“`Thread`”来运行您的耗时任务，并使用另一个“`Thread`”来相应地更新进度条状态。阅读代码的注释，应该是不言自明的。

*文件:MyAndroidAppActivity.java*

```java
 package com.mkyong.android;

import android.app.Activity;
import android.app.ProgressDialog;
import android.os.Bundle;
import android.os.Handler;
import android.widget.Button;
import android.view.View;
import android.view.View.OnClickListener;

public class MyAndroidAppActivity extends Activity {

	Button btnStartProgress;
	ProgressDialog progressBar;
	private int progressBarStatus = 0;
	private Handler progressBarHandler = new Handler();

	private long fileSize = 0;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		addListenerOnButton();

	}

	public void addListenerOnButton() {

		btnStartProgress = (Button) findViewById(R.id.btnStartProgress);
		btnStartProgress.setOnClickListener(
                 new OnClickListener() {

		   @Override
		   public void onClick(View v) {

			// prepare for a progress bar dialog
			progressBar = new ProgressDialog(v.getContext());
			progressBar.setCancelable(true);
			progressBar.setMessage("File downloading ...");
			progressBar.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
			progressBar.setProgress(0);
			progressBar.setMax(100);
			progressBar.show();

			//reset progress bar status
			progressBarStatus = 0;

			//reset filesize
			fileSize = 0;

			new Thread(new Runnable() {
			  public void run() {
				while (progressBarStatus < 100) {

				  // process some tasks
				  progressBarStatus = doSomeTasks();

				  // your computer is too fast, sleep 1 second
				  try {
					Thread.sleep(1000);
				  } catch (InterruptedException e) {
					e.printStackTrace();
				  }

				  // Update the progress bar
				  progressBarHandler.post(new Runnable() {
					public void run() {
					  progressBar.setProgress(progressBarStatus);
					}
				  });
				}

				// ok, file is downloaded,
				if (progressBarStatus >= 100) {

					// sleep 2 seconds, so that you can see the 100%
					try {
						Thread.sleep(2000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}

					// close the progress bar dialog
					progressBar.dismiss();
				}
			  }
		       }).start();

	           }

                });

        }

	// file download simulator... a really simple
	public int doSomeTasks() {

		while (fileSize <= 1000000) {

			fileSize++;

			if (fileSize == 100000) {
				return 10;
			} else if (fileSize == 200000) {
				return 20;
			} else if (fileSize == 300000) {
				return 30;
			}
			// ...add your own

		}

		return 100;

	}

} 
```

“doSomeTasks”方法只是一个文件大小下载模拟器，只是用你的长时间运行的任务替换这个方法。

 ## 3.演示

运行应用程序。

1.结果，一个按钮。

![android progress bar demo1](img/d0129d436368272cd7c1c2cbf7f3c2e1.png "android-progressbar-demo1")

2.点击按钮，会提示一个“进度条对话框”显示当前下载进度。

![android progress bar demo2](img/d80921c5c3ead8693927bc979b75454f.png "android-progressbar-demo2")

3.任务完成后，进度条会显示 100%，并自动关闭。

![android progress bar demo3](img/ab100c130ad3a2772061ac009926e89f.png "android-progressbar-demo3")

## 下载源代码

Download it - [Android-ProgressBar-Example.zip](http://web.archive.org/web/20190210022552/http://www.mkyong.com/wp-content/uploads/2011/12/Android-ProgressBar-Example.zip) (15 KB)

## 参考

1.  [安卓进度条示例](http://web.archive.org/web/20190210022552/http://developer.android.com/reference/android/widget/ProgressBar.html)
2.  [Android progress dialog JavaDoc](http://web.archive.org/web/20190210022552/http://developer.android.com/reference/android/app/ProgressDialog.html)
3.  [另一个 Android ProgressBar 例子](http://web.archive.org/web/20190210022552/http://huuah.com/android-progress-bar-and-thread-updating/)

[android](http://web.archive.org/web/20190210022552/http://www.mkyong.com/tag/android/) [progress bar](http://web.archive.org/web/20190210022552/http://www.mkyong.com/tag/progress-bar/)







