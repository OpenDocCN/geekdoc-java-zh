# Java 8–无法从 TemporalAccessor 获取本地日期时间

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-unable-to-obtain-localdatetime-from-temporalaccessor/>

一个将字符串转换为`LocalDateTime`的例子，但是它提示以下错误:

Java8Example.java

```java
 package com.mkyong.demo;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Locale;

public class Java8Example {

    public static void main(String[] args) {

        String str = "31-Aug-2020";

        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("dd-MMM-yyyy", Locale.US);

        LocalDateTime localDateTime = LocalDateTime.parse(str, dtf);

        System.out.println(localDateTime);

    }
} 
```

输出

```java
 Exception in thread "main" java.time.format.DateTimeParseException: Text '31-Aug-2020' could not be parsed: Unable to obtain LocalDateTime from TemporalAccessor: {},ISO resolved to 2020-08-31 of type java.time.format.Parsed
	at java.base/java.time.format.DateTimeFormatter.createError(DateTimeFormatter.java:2017)
	at java.base/java.time.format.DateTimeFormatter.parse(DateTimeFormatter.java:1952)
	at java.base/java.time.LocalDateTime.parse(LocalDateTime.java:492)
	at com.mkyong.demo.Java8Example.main(Java8Example.java:15)
Caused by: java.time.DateTimeException: Unable to obtain LocalDateTime from TemporalAccessor: {},ISO resolved to 2020-08-31 of type java.time.format.Parsed
	at java.base/java.time.LocalDateTime.from(LocalDateTime.java:461)
	at java.base/java.time.format.Parsed.query(Parsed.java:235)
	at java.base/java.time.format.DateTimeFormatter.parse(DateTimeFormatter.java:1948)
	... 2 more
Caused by: java.time.DateTimeException: Unable to obtain LocalTime from TemporalAccessor: {},ISO resolved to 2020-08-31 of type java.time.format.Parsed
	at java.base/java.time.LocalTime.from(LocalTime.java:431)
	at java.base/java.time.LocalDateTime.from(LocalDateTime.java:457)
	... 4 more 
```

## 解决办法

日期`31-Aug-2020`不包含时间，为了解决这个问题，使用`LocalDate.parse(str, dtf).atStartOfDay()`

```java
 String str = "31-Aug-2020";

  DateTimeFormatter dtf = DateTimeFormatter.ofPattern("dd-MMM-yyyy", Locale.US);

  LocalDateTime localDateTime = LocalDate.parse(str, dtf).atStartOfDay(); 
```

## 参考

*   [DateTimeFormatter JavaDoc](http://web.archive.org/web/20221207132857/https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#patterns)
*   [本地日期时间 JavaDoc](http://web.archive.org/web/20221207132857/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)
*   [Java 8–如何用 LocalDateTime 解析日期](/web/20221207132857/https://mkyong.com/java8/java-8-how-to-parse-date-with-localdatetime/)
*   [Java 8–如何将字符串转换为本地日期](/web/20221207132857/https://mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)

<input type="hidden" id="mkyong-current-postId" value="15372">