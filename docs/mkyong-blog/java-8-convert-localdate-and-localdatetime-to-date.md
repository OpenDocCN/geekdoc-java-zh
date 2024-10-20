# Java 8–将本地日期和本地日期时间转换为日期

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-localdate-and-localdatetime-to-date/>

一个将 Java 8 `java.time.LocalDate`和`java.time.LocalDateTime`转换回经典`java.uti.Date`的 Java 例子。

JavaDateExample.java

```java
 package com.mkyong.time;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.util.Date;

public class JavaDateExample {

    public static void main(String[] args) {

        // LocalDate -> Date
        LocalDate localDate = LocalDate.of(2020, 2, 20);
        Date date = Date.from(localDate.atStartOfDay(ZoneId.systemDefault()).toInstant());

        // LocalDateTime -> Date
        LocalDateTime localDateTime = LocalDateTime.of(2020,2,20,21,46,31);
        Date date2 = Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());

        // ZonedDateTime -> Date
        ZonedDateTime zonedDateTime = localDateTime.atZone(ZoneId.systemDefault());
        Date date3 = Date.from(zonedDateTime.toInstant());

    }

} 
```

输出

```java
 Thu Feb 20 00:00:00 MYT 2020
Thu Feb 20 21:46:31 MYT 2020
Thu Feb 20 21:46:31 MYT 2020 
```

**Note**
More examples – [Java 8 – Convert Date to LocalDate and LocalDateTime](/web/20221027024411/https://mkyong.com/java8/java-8-convert-date-to-localdate-and-localdatetime/)

## 参考

*   [title =]Java 8–将日期转换为本地日期和本地日期时间](/Java 8/Java-8-Convert-Date-to-local Date-and-local datetime/)
*   [本地日期 JavaDoc](http://web.archive.org/web/20221027024411/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html)
*   [本地日期时间 JavaDoc](http://web.archive.org/web/20221027024411/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)

<input type="hidden" id="mkyong-current-postId" value="15384">