# Java——如何给当前日期添加天数

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-add-days-to-current-date/>

本文向您展示了如何使用传统的`java.util.Calendar`和新的 Java 8 日期和时间 API 向当前日期添加天数。

## 1.Calendar.add

向当前日期添加 1 年 1 个月 1 天 1 小时 1 分 1 秒的示例。

DateExample.java

```java
 package com.mkyong.time;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

public class DateExample {

    private static final DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");

    public static void main(String[] args) {

        Date currentDate = new Date();
        System.out.println(dateFormat.format(currentDate));

        // convert date to calendar
        Calendar c = Calendar.getInstance();
        c.setTime(currentDate);

        // manipulate date
        c.add(Calendar.YEAR, 1);
        c.add(Calendar.MONTH, 1);
        c.add(Calendar.DATE, 1); //same with c.add(Calendar.DAY_OF_MONTH, 1);
        c.add(Calendar.HOUR, 1);
        c.add(Calendar.MINUTE, 1);
        c.add(Calendar.SECOND, 1);

        // convert calendar to date
        Date currentDatePlusOne = c.getTime();

        System.out.println(dateFormat.format(currentDatePlusOne));

    }

} 
```

输出

```java
 2016/11/10 17:11:48
2017/12/11 18:12:49 
```

## 2.Java 8 加减

在 Java 8 中，您可以使用加号和减号方法来操作 LocalDate、LocalDateTime 和 ZoneDateTime，请参见以下示例

LocalDateTimeExample.java

```java
 package com.mkyong.time;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.util.Date;

public class LocalDateTimeExample {

    private static final String DATE_FORMAT = "yyyy/MM/dd HH:mm:ss";
    private static final DateFormat dateFormat = new SimpleDateFormat(DATE_FORMAT);
    private static final DateTimeFormatter dateFormat8 = DateTimeFormatter.ofPattern(DATE_FORMAT);

    public static void main(String[] args) {

		// Get current date
        Date currentDate = new Date();
        System.out.println("date : " + dateFormat.format(currentDate));

        // convert date to localdatetime
        LocalDateTime localDateTime = currentDate.toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
        System.out.println("localDateTime : " + dateFormat8.format(localDateTime));

        // plus one
        localDateTime = localDateTime.plusYears(1).plusMonths(1).plusDays(1);
        localDateTime = localDateTime.plusHours(1).plusMinutes(2).minusMinutes(1).plusSeconds(1);

        // convert LocalDateTime to date
        Date currentDatePlusOneDay = Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());

        System.out.println("\nOutput : " + dateFormat.format(currentDatePlusOneDay));

    }

} 
```

输出

```java
 date : 2016/11/10 17:40:11
localDateTime : 2016/11/10 17:40:11

Output : 2017/12/11 18:41:12 
```

## 参考

1.  [日历 JavaDoc](http://web.archive.org/web/20221006190628/https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html)
2.  [Java–如何获取当前日期时间–date()和 calendar()](http://web.archive.org/web/20221006190628/https://www.mkyong.com/java/java-how-to-get-current-date-time-date-and-calender/)
3.  [Java 8–将日期转换为本地日期和本地日期时间](http://web.archive.org/web/20221006190628/https://www.mkyong.com/java8/java-8-convert-date-to-localdate-and-localdatetime/)

<input type="hidden" id="mkyong-current-postId" value="14073">