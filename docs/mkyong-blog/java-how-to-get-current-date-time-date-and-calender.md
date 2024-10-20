# Java——如何获取当前日期时间

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-get-current-date-time-date-and-calender/>

在本教程中，我们将向您展示如何从新的 Java 8 `java.time.*`中获取当前日期时间，如 [Localdate](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalDate.html) 、 [LocalTime](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalTime.html) 、 [LocalDateTime](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalDateTime.html) 、 [ZonedDateTime](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/ZonedDateTime.html) 、 [Instant](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/Instant.html) ，以及旧的日期时间 API，如 [Date](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Date.html) 和 [Calendar](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Calendar.html) 。

目录

*   [1。用 Java 获取当前日期时间](#get-current-date-time-in-java)
*   [2 .java.time.LocalDate](#javatimelocaldate)
*   [3。java.time.LocalTime](#javatimelocaltime)
*   [4 .Java . time . localdatetime](#javatimelocaldatetime)
*   [5\. java.time.ZonedDateTime](#javatimezoneddatetime)
*   [6。java.time.Instant](#javatimeinstant)
*   [7。java.util.Date(旧版)](#javautildate-legacy)
*   [8。java.util.Calendar(旧版)](#javautilcalendar-legacy)
*   [9。参考文献](#references)

摘要

*   对于新的 Java 8`java.time.*`API，我们可以使用`.now()`获取当前的日期时间，并用 [DateTimeFormatter](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/format/DateTimeFormatter.html) 对其进行格式化。
*   对于遗留的日期时间 API，我们可以使用`new Date()`和`Calendar.getInstance()`来获取当前的日期时间，并用 [SimpleDateFormat](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/SimpleDateFormat.html) 对其进行格式化。

## 1。用 Java 获取当前日期时间

下面是一些用 Java 显示当前日期时间的代码片段。

对于`java.time.LocalDate`，使用`LocalDate.now()`

```java
 DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu/MM/dd");
  LocalDate localDate = LocalDate.now();
  System.out.println(dtf.format(localDate));            // 2021/03/22 
```

对于`java.time.localTime`，使用`LocalTime.now()`

```java
 DateTimeFormatter dtf = DateTimeFormatter.ofPattern("HH:mm:ss");
  LocalTime localTime = LocalTime.now();
  System.out.println(dtf.format(localTime));            // 16:37:15 
```

对于`java.time.LocalDateTime`，使用`LocalDateTime.now()`

```java
 DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu/MM/dd HH:mm:ss");
  LocalDateTime now = LocalDateTime.now();
  System.out.println(dtf.format(now));                  //  2021/03/22 16:37:15 
```

对于`java.time.ZonedDateTime`，使用`ZonedDateTime.now()`

```java
 // get current date-time, with system default time zone
  DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu/MM/dd HH:mm:ss");
  ZonedDateTime now = ZonedDateTime.now();
  System.out.println(dtf.format(now));                  // 2021/03/22 16:37:15
  System.out.println(now.getOffset());                  // +08:00

  // get current date-time, with a specified time zone
  ZonedDateTime japanDateTime = now.withZoneSameInstant(ZoneId.of("Asia/Tokyo"));
  System.out.println(dtf.format(japanDateTime));        // 2021/03/22 17:37:15
  System.out.println(japanDateTime.getOffset());        // +09:00 
```

对于`java.time.Instant`，使用`Instant.now()`

```java
 Instant now = Instant.now();

  // convert Instant to ZonedDateTime
  DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu/MM/dd HH:mm:ss");
  ZonedDateTime zonedDateTime = ZonedDateTime.ofInstant(now, ZoneId.systemDefault());
  System.out.println(dtfDateTime.format(zonedDateTime)); 
```

对于`java.util.Date`，使用`new Date()`

```java
 DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
  Date date = new Date();
  System.out.println(dateFormat.format(date));           // 2021/03/22 16:37:15 
```

对于`java.util.Calendar`，使用`Calendar.getInstance()`

```java
 DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
  Calendar cal = Calendar.getInstance();
  System.out.println(dateFormat.format(cal.getTime()));  // 2021/03/22 16:37:15 
```

## 2 .java.time.LocalDate

对于`java.time.LocalDate`，使用`LocalDate.now()`获取不带时区的当前日期，并用`DateTimeFormatter`对其进行格式化。

LocalDateExample.java

```java
 package com.mkyong.app;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class LocalDateExample {

  public static void main(String[] args) {

      DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu/MM/dd");
      LocalDate localDate = LocalDate.now();
      System.out.println(dtf.format(localDate));    // 2021/03/22

  }

} 
```

输出

Terminal

```java
 2021/03/22 
```

## 3。java.time.LocalTime

对于`java.time.LocalTime`，使用`LocalDate.now()`获取不带时区的当前时间，并用`DateTimeFormatter`对其进行格式化。

LocalTimeExample.java

```java
 package com.mkyong.app;

import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

public class LocalTimeExample {

    public static void main(String[] args) {

        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("HH:mm:ss");
        LocalTime localTime = LocalTime.now();
        System.out.println(dtf.format(localTime));    // 16:37:15

    }

} 
```

输出

Terminal

```java
 16:37:15 
```

## 4 .Java . time . localdatetime

对于`java.time.LocalDateTime`，使用`LocalDateTime.now()`获取不带时区的当前日期时间，并用`DateTimeFormatter`对其进行格式化。

LocalDateTimeExample.java

```java
 package com.mkyong.app;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class LocalDateTimeExample {

    public static void main(String[] args) {

        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu/MM/dd HH:mm:ss");
        LocalDateTime now = LocalDateTime.now();
        System.out.println(dtf.format(now));        //  2021/03/22 16:37:15

    }

} 
```

输出

Terminal

```java
 2021/03/22 16:37:15 
```

## 5\. java.time.ZonedDateTime

对于`java.time.ZonedDateTime`，使用`ZonedDateTime.now()`获取系统默认时区或指定时区的当前日期时间。

ZonedDateTimeExample.java

```java
 package com.mkyong.app;

import java.time.OffsetDateTime;
import java.time.ZoneId;
import java.time.ZoneOffset;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;

public class ZonedDateTimeExample {

  public static void main(String[] args) {

      DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu/MM/dd HH:mm:ss");

      // Get default time zone
      System.out.println(ZoneOffset.systemDefault());         // Asia/Kuala_Lumpur
      System.out.println(OffsetDateTime.now().getOffset());   // +08:00

      // get current date time, with +08:00
      ZonedDateTime now = ZonedDateTime.now();
      System.out.println(dtf.format(now));                    // 2021/03/22 16:37:15
      System.out.println(now.getOffset());                    // +08:00

      // get get current date time, with +09:00
      ZonedDateTime japanDateTime = now.withZoneSameInstant(ZoneId.of("Asia/Tokyo"));
      System.out.println(dtf.format(japanDateTime));          // 2021/03/22 17:37:15
      System.out.println(japanDateTime.getOffset());          // +09:00

  }

} 
```

输出

Terminal

```java
 Asia/Kuala_Lumpur
+08:00

2021/03/22 16:37:15
+08:00

2021/03/22 17:37:15
+09:00 
```

## 6。java.time.Instant

对于`java.time.Instant`，使用`Instant.now()`获取自 [Unix 纪元时间](http://web.archive.org/web/20221123152829/https://en.wikipedia.org/wiki/Unix_time)(UTC 1970 年 1 月 1 日午夜)以来经过的秒数，然后转换为其他`java.time.*`日期时间类，如`LocalDate`、`LocalDateTime`和`ZonedDateTime`。

InstantExample.java

```java
 package com.mkyong.app;

import java.time.*;
import java.time.format.DateTimeFormatter;

public class InstantExample {

  private static final DateTimeFormatter dtfDate = DateTimeFormatter.ofPattern("uuuu/MM/dd");
  private static final DateTimeFormatter dtfTime = DateTimeFormatter.ofPattern("HH:mm:ss");
  private static final DateTimeFormatter dtfDateTime = DateTimeFormatter.ofPattern("uuuu/MM/dd HH:mm:ss");

  public static void main(String[] args) {

      // seconds passed since the Unix epoch time (midnight of January 1, 1970 UTC)
      Instant now = Instant.now();

      // convert Instant to LocalDate
      LocalDate localDate = LocalDate.ofInstant(now, ZoneId.systemDefault());
      System.out.println(dtfDate.format(localDate));

      // convert Instant to localTime
      LocalTime localTime = LocalTime.ofInstant(now, ZoneId.systemDefault());
      System.out.println(dtfTime.format(localTime));

      // convert Instant to LocalDateTime
      LocalDateTime localDateTime = LocalDateTime.ofInstant(now, ZoneId.systemDefault());
      System.out.println(dtfDateTime.format(localDateTime));

      // convert Instant to ZonedDateTime
      ZonedDateTime zonedDateTime = ZonedDateTime.ofInstant(now, ZoneId.systemDefault());
      System.out.println(dtfDateTime.format(zonedDateTime));

  }

} 
```

输出

Terminal

```java
 2021/03/22
16:37:15
2021/03/22 16:37:15
2021/03/22 16:37:15 
```

## 7。java.util.Date(旧版)

对于传统的`java.util.Date`，使用`new Date()`或`new Date(System.currentTimeMillis()`获取当前日期时间，并用`SimpleDateFormat`对其进行格式化。

DateExample.java

```java
 package com.mkyong.app;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateExample {

  public static void main(String[] args) {

      DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");

      Date date = new Date();
      System.out.println(dateFormat.format(date));    // 2021/03/22 16:37:15

      // new Date() actually calls this new Date(long date)
      Date date2 = new Date(System.currentTimeMillis());
      System.out.println(dateFormat.format(date));    // 2021/03/22 16:37:15

  }

} 
```

输出

Terminal

```java
 2021/03/22 16:37:15
  2021/03/22 16:37:15 
```

## 8。java.util.Calendar(旧版)

对于传统的`java.util.Calendar`，使用`Calendar.getInstance()`获取当前日期时间，并用`SimpleDateFormat`对其进行格式化。

CalendarExample.java

```java
 package com.mkyong.app;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Calendar;

public class CalendarExample {

  public static void main(String[] args) {

      DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
      Calendar cal = Calendar.getInstance();
      System.out.println(dateFormat.format(cal.getTime()));   // 2021/03/22 16:37:15

  }

} 
```

输出

Terminal

```java
 2021/03/22 16:37:15 
```

## 9。参考文献

*   [stack overflow-`uuuu`与 Java 中`DateTimeFormatter`格式中的`yyyy`？](http://web.archive.org/web/20221123152829/https://stackoverflow.com/questions/41177442/uuuu-versus-yyyy-in-datetimeformatter-formatting-pattern-codes-in-java)
*   [Java 日期时间教程](http://web.archive.org/web/20221123152829/https://mkyong.com/tutorials/java-date-time-tutorials/)
*   [如何在 Java 中获取当前时间戳](http://web.archive.org/web/20221123152829/https://mkyong.com/java/how-to-get-current-timestamps-in-java)
*   [维基百科–纪元时间](http://web.archive.org/web/20221123152829/https://en.wikipedia.org/wiki/Unix_time)
*   [局部日期](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalDate.html)
*   [当地时间](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalTime.html)
*   [局部日期时间](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalDateTime.html)
*   [ZonedDateTime](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/ZonedDateTime.html)
*   [瞬间](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/Instant.html)
*   [日期](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Date.html)
*   [日历](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Calendar.html)
*   [DateTimeFormatter](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/format/DateTimeFormatter.html)
*   [简单日期格式](http://web.archive.org/web/20221123152829/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/SimpleDateFormat.html)

<input type="hidden" id="mkyong-current-postId" value="203">