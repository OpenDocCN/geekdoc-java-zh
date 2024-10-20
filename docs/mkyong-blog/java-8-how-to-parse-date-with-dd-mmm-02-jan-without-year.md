# Java 8–如何解析带有“DD MMM”(02 Jan)的日期，而不带年份？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-parse-date-with-dd-mmm-02-jan-without-year/>

这个例子展示了如何解析一个没有指定年份的日期(02 Jan)。

JavaDateExample.java

```java
 package com.mkyong.time;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.Locale;

public class JavaDateExample {

    public static void main(String[] args) {

        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd MMM", Locale.US);

        String date = "02 Jan";

        LocalDate localDate = LocalDate.parse(date, formatter);

        System.out.println(localDate);

        System.out.println(formatter.format(localDate));

    }

} 
```

输出

```java
 Exception in thread "main" java.time.format.DateTimeParseException: Text '02 Jan' could not be parsed: Unable to obtain LocalDate from TemporalAccessor: {DayOfMonth=2, MonthOfYear=1},ISO of type java.time.format.Parsed
	at java.base/java.time.format.DateTimeFormatter.createError(DateTimeFormatter.java:2017)
	at java.base/java.time.format.DateTimeFormatter.parse(DateTimeFormatter.java:1952)
	at java.base/java.time.LocalDate.parse(LocalDate.java:428)
	at com.mkyong.time.JavaDateExample.main(JavaDateExample.java:20)
Caused by: java.time.DateTimeException: Unable to obtain LocalDate from TemporalAccessor: {DayOfMonth=2, MonthOfYear=1},ISO of type java.time.format.Parsed
	at java.base/java.time.LocalDate.from(LocalDate.java:396)
	at java.base/java.time.format.Parsed.query(Parsed.java:235)
	at java.base/java.time.format.DateTimeFormatter.parse(DateTimeFormatter.java:1948)
	... 2 more 
```

## 解决办法

格局`dd MMM`不够；我们需要一个`DateTimeFormatterBuilder`来为日期解析提供默认年份。

JavaDateExample.java

```java
 package com.mkyong.time;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeFormatterBuilder;
import java.time.temporal.ChronoField;
import java.util.Locale;

public class JavaDateExample {

    public static void main(String[] args) {

        DateTimeFormatter formatter = new DateTimeFormatterBuilder()
                .appendPattern("dd MMM")
                .parseDefaulting(ChronoField.YEAR, 2020)
                .toFormatter(Locale.US);

        String date = "02 Jan";

        LocalDate localDate = LocalDate.parse(date, formatter);

        System.out.println(localDate);

        System.out.println(formatter.format(localDate));

    }

} 
```

输出

```java
 2020-01-02
02 Jan 
```

## 参考

*   [Java 8–如何将字符串转换为本地日期](/web/20221205141223/https://mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)
*   [DateTimeFormatterBuilder JavaDoc](http://web.archive.org/web/20221205141223/https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatterBuilder.html)

<input type="hidden" id="mkyong-current-postId" value="15378">