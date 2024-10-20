# Android 日期选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-date-picker-example/>

在 Android 中，您可以使用“[Android . widget . date picker](http://web.archive.org/web/20210506220718/https://developer.android.com/reference/android/widget/DatePicker.html)”类来呈现一个日期选择器组件，以便在预定义的用户界面中选择日、月和年。

在本教程中，我们将向您展示如何通过[Android . widget . date picker](http://web.archive.org/web/20210506220718/https://developer.android.com/reference/android/widget/DatePicker.html)在当前页面中呈现日期选择器组件，以及通过[Android . app . datepickerdialog](http://web.archive.org/web/20210506220718/https://developer.android.com/reference/android/app/DatePickerDialog.html)在对话框中呈现日期选择器组件。此外，我们还向您展示了如何在日期选择器组件中设置日期。

*P.S 这个项目是在 Eclipse 3.7 中开发的，用 Android 2.3.3 测试过。*

## 1.日期选择器

打开“ **res/layout/main.xml** ”文件，添加日期选择器、标签和按钮进行演示。

*文件:res/layout/main.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Button
        android:id="@+id/btnChangeDate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Change Date" />

    <TextView
        android:id="@+id/lblDate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Current Date (M-D-YYYY): "
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <TextView
        android:id="@+id/tvDate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <DatePicker
        android:id="@+id/dpResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</LinearLayout> 
```

"`DatePickerDialog`"是在代码中声明，而不是在 XML 中。

## 2.代码代码

阅读代码的注释，应该是不言自明的。

*文件:MyAndroidAppActivity.java*

```java
 package com.mkyong.android;

import java.util.Calendar;
import android.app.Activity;
import android.app.DatePickerDialog;
import android.app.Dialog;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.DatePicker;
import android.widget.TextView;

public class MyAndroidAppActivity extends Activity {

	private TextView tvDisplayDate;
	private DatePicker dpResult;
	private Button btnChangeDate;

	private int year;
	private int month;
	private int day;

	static final int DATE_DIALOG_ID = 999;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		setCurrentDateOnView();
		addListenerOnButton();

	}

	// display current date
	public void setCurrentDateOnView() {

		tvDisplayDate = (TextView) findViewById(R.id.tvDate);
		dpResult = (DatePicker) findViewById(R.id.dpResult);

		final Calendar c = Calendar.getInstance();
		year = c.get(Calendar.YEAR);
		month = c.get(Calendar.MONTH);
		day = c.get(Calendar.DAY_OF_MONTH);

		// set current date into textview
		tvDisplayDate.setText(new StringBuilder()
			// Month is 0 based, just add 1
			.append(month + 1).append("-").append(day).append("-")
			.append(year).append(" "));

		// set current date into datepicker
		dpResult.init(year, month, day, null);

	}

	public void addListenerOnButton() {

		btnChangeDate = (Button) findViewById(R.id.btnChangeDate);

		btnChangeDate.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {

				showDialog(DATE_DIALOG_ID);

			}

		});

	}

	@Override
	protected Dialog onCreateDialog(int id) {
		switch (id) {
		case DATE_DIALOG_ID:
		   // set date picker as current date
		   return new DatePickerDialog(this, datePickerListener, 
                         year, month,day);
		}
		return null;
	}

	private DatePickerDialog.OnDateSetListener datePickerListener 
                = new DatePickerDialog.OnDateSetListener() {

		// when dialog box is closed, below method will be called.
		public void onDateSet(DatePicker view, int selectedYear,
				int selectedMonth, int selectedDay) {
			year = selectedYear;
			month = selectedMonth;
			day = selectedDay;

			// set selected date into textview
			tvDisplayDate.setText(new StringBuilder().append(month + 1)
			   .append("-").append(day).append("-").append(year)
			   .append(" "));

			// set selected date into datepicker also
			dpResult.init(year, month, day, null);

		}
	};

} 
```

上面的“DatePickerDialog”例子是从[谷歌 Android 日期选择器例子](http://web.archive.org/web/20210506220718/https://developer.android.com/resources/tutorials/views/hello-datepicker.html) 引用的，有一些小的变化。

## 3.演示

运行应用程序。

1.结果，“日期选择器”和“文本视图”被设置为当前日期。

![android datepicker demo1](img/c580353f701d61e027002af78872106c.png "android-datepicker-demo1")

2.点击“更改日期”按钮，将通过`DatePickerDialog`在对话框中提示一个日期选择器组件。

![android datepicker demo2](img/0c23a0f8a05013f1de957b490bc0db4f.png "android-datepicker-demo2")

3.“日期选择器”和“文本视图”都用选定的日期更新。

![android datepicker demo3](img/0a302665233cca3c4026fb93f9a5daa0.png "android-datepicker-demo3")

## 下载源代码

Download it – [Android-DatePicker-Example.zip](http://web.archive.org/web/20210506220718/http://www.mkyong.com/wp-content/uploads/2011/11/Android-DatePicker-Example.zip) (16 KB)

## 参考

1.  [Android 日期选择器示例](http://web.archive.org/web/20210506220718/https://developer.android.com/resources/tutorials/views/hello-datepicker.html)
2.  [Android DatePicker JavaDoc](http://web.archive.org/web/20210506220718/https://developer.android.com/reference/android/widget/DatePicker.html)
3.  [Android datepicker 对话框 javadoc】t1](http://web.archive.org/web/20210506220718/https://developer.android.com/reference/android/app/DatePickerDialog.html)

Tags : [android](http://web.archive.org/web/20210506220718/https://mkyong.com/tag/android/) [date](http://web.archive.org/web/20210506220718/https://mkyong.com/tag/date/) [date picker](http://web.archive.org/web/20210506220718/https://mkyong.com/tag/date-picker/)<input type="hidden" id="mkyong-current-postId" value="10255">