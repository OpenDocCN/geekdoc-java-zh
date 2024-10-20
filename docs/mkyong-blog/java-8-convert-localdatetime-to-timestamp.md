# Java 8–将本地日期时间转换为时间戳

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-localdatetime-to-timestamp/>

在 Java 中，我们可以使用`Timestamp.valueOf(LocalDateTime)`将一个`LocalDateTime`转换成一个`Timestamp`。

## 1.localdatetime〔t0〕timestamp

Java 示例将`java.time.LocalDateTime`转换为`java.sql.Timestamp`，反之亦然。

TimeExample.java

```java
 package com.mkyong;

import java.sql.Timestamp;
import java.time.LocalDateTime;

public class TimeExample {

    public static void main(String[] args) {

        //  LocalDateTime to Timestamp
        LocalDateTime now = LocalDateTime.now();
        Timestamp timestamp = Timestamp.valueOf(now);

        System.out.println(now);            // 2019-06-14T15:50:36.068076300
        System.out.println(timestamp);      // 2019-06-14 15:50:36.0680763

        //  Timestamp to LocalDateTime
        LocalDateTime localDateTime = timestamp.toLocalDateTime();

        System.out.println(localDateTime);  // 2019-06-14T15:50:36.068076300

    }
} 
```

## 2.LocalDate Timestamp

如题。

TimeExample2.java

```java
 package com.mkyong.jdbc;

import java.sql.Timestamp;
import java.time.LocalDate;

public class TimeExample2 {

    public static void main(String[] args) {

        //  LocalDate to Timestamp
        LocalDate now = LocalDate.now();
        Timestamp timestamp = Timestamp.valueOf(now.atStartOfDay());

        System.out.println(now);        // 2019-06-14
        System.out.println(timestamp);  // 2019-06-14 00:00:00.0

        //  Timestamp to LocalDate
        LocalDate localDate = timestamp.toLocalDateTime().toLocalDate();
        System.out.println(localDate);  // 2019-06-14

    }
} 
```

# 参考

*   [Java 8–如何格式化本地日期时间](http://web.archive.org/web/20210815051316/https://www.mkyong.com/java8/java-8-how-to-format-localdatetime/)
*   [LocalDate JavaDocs](http://web.archive.org/web/20210815051316/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html)
*   [LocalDateTime JavaDocs](http://web.archive.org/web/20210815051316/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)
*   [时间戳 JavaDocs](http://web.archive.org/web/20210815051316/https://docs.oracle.com/javase/8/docs/api/java/sql/Timestamp.html)

Tags : [date conversion](http://web.archive.org/web/20210815051316/https://mkyong.com/tag/date-conversion/) [java 8](http://web.archive.org/web/20210815051316/https://mkyong.com/tag/java-8/) [java.time](http://web.archive.org/web/20210815051316/https://mkyong.com/tag/java-time/)<input type="hidden" id="mkyong-current-postId" value="15125">