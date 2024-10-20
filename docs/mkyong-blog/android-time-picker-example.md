# Android 时间选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-time-picker-example/>

在 Android 中，您可以使用"[Android . widget . time picker](http://web.archive.org/web/20221023054402/https://developer.android.com/reference/android/widget/TimePicker.html)"类来呈现时间选择器组件，以便在预定义的用户界面中选择小时和分钟。

在本教程中，我们将向您展示如何通过[Android . widget . time picker](http://web.archive.org/web/20221023054402/https://developer.android.com/reference/android/widget/TimePicker.html)在当前页面中呈现时间选择器组件，以及通过[Android . app . time picker dialog](http://web.archive.org/web/20221023054402/https://developer.android.com/reference/android/app/TimePickerDialog.html)在对话框中呈现时间选择器组件。此外，我们还向您展示了如何设置时间选择器组件中的小时和分钟。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

## 1.时间选择器

打开“ **res/layout/main.xml** ”文件，添加时间选择器、标签和按钮进行演示。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Button
        android:id="@+id/btnChangeTime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Change Time" />

    <TextView
        android:id="@+id/lblTime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Current Time (H:M): "
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <TextView
        android:id="@+id/tvTime"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <TimePicker
        android:id="@+id/timePicker1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</LinearLayout> 
```

"`TimePickerDialog`"是在代码中声明，而不是在 XML 中。

## 2.代码代码

阅读代码的注释，应该是不言自明的。

*文件:MyAndroidAppActivity.java*

```java
 package com.mkyong.android;

import java.util.Calendar;
import android.app.Activity;
import android.app.Dialog;
import android.app.TimePickerDialog;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.TextView;
import android.widget.TimePicker;

public class MyAndroidAppActivity extends Activity {

	private TextView tvDisplayTime;
	private TimePicker timePicker1;
	private Button btnChangeTime;

	private int hour;
	private int minute;

	static final int TIME_DIALOG_ID = 999;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		setCurrentTimeOnView();
		addListenerOnButton();

	}

	// display current time
	public void setCurrentTimeOnView() {

		tvDisplayTime = (TextView) findViewById(R.id.tvTime);
		timePicker1 = (TimePicker) findViewById(R.id.timePicker1);

		final Calendar c = Calendar.getInstance();
		hour = c.get(Calendar.HOUR_OF_DAY);
		minute = c.get(Calendar.MINUTE);

		// set current time into textview
		tvDisplayTime.setText(
                    new StringBuilder().append(pad(hour))
                                       .append(":").append(pad(minute)));

		// set current time into timepicker
		timePicker1.setCurrentHour(hour);
		timePicker1.setCurrentMinute(minute);

	}

	public void addListenerOnButton() {

		btnChangeTime = (Button) findViewById(R.id.btnChangeTime);

		btnChangeTime.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {

				showDialog(TIME_DIALOG_ID);

			}

		});

	}

	@Override
	protected Dialog onCreateDialog(int id) {
		switch (id) {
		case TIME_DIALOG_ID:
			// set time picker as current time
			return new TimePickerDialog(this, 
                                        timePickerListener, hour, minute,false);

		}
		return null;
	}

	private TimePickerDialog.OnTimeSetListener timePickerListener = 
            new TimePickerDialog.OnTimeSetListener() {
		public void onTimeSet(TimePicker view, int selectedHour,
				int selectedMinute) {
			hour = selectedHour;
			minute = selectedMinute;

			// set current time into textview
			tvDisplayTime.setText(new StringBuilder().append(pad(hour))
					.append(":").append(pad(minute)));

			// set current time into timepicker
			timePicker1.setCurrentHour(hour);
			timePicker1.setCurrentMinute(minute);

		}
	};

	private static String pad(int c) {
		if (c >= 10)
		   return String.valueOf(c);
		else
		   return "0" + String.valueOf(c);
	}
} 
```

*另外，上面的“时间选择器对话框”的例子是从[谷歌安卓时间选择器的例子](http://web.archive.org/web/20221023054402/https://developer.android.com/resources/tutorials/views/hello-timepicker.html)、*中引用的，有一些小的变化。

## 3.演示

运行应用程序。

1.结果，“时间选择器”和“文本视图”被设置为当前时间。

![android timepicker demo1](img/77d021750889080447cacda0d107aab9.png "android-timepicker-demo1")

2.点击“更改时间”按钮，将通过`TimePickerDialog`在对话框中提示一个时间选择器组件。

![android timepicker demo2](img/76e4fd3f6357a45400cce594c39de6b5.png "android-timepicker-demo2")

3.“时间选择器”和“文本视图”都用选定的时间更新。

![android timepicker demo3](img/5bcf7b5a84d0cf2d7e66630243545bdd.png "android-timepicker-demo3")

## 下载源代码

Download it – [Android-TimePicker-Example.zip](http://web.archive.org/web/20221023054402/http://www.mkyong.com/wp-content/uploads/2011/11/Android-TimePicker-Example.zip) (16 KB)

## 参考

1.  [Android 时间选择器示例](http://web.archive.org/web/20221023054402/https://developer.android.com/resources/tutorials/views/hello-timepicker.html)
2.  [Android TimePicker JavaDoc](http://web.archive.org/web/20221023054402/https://developer.android.com/reference/android/widget/TimePicker.html)
3.  [Android time picker dialog JavaDoc](http://web.archive.org/web/20221023054402/https://developer.android.com/reference/android/app/TimePickerDialog.html)

<input type="hidden" id="mkyong-current-postId" value="10267">