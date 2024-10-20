# Java 8–将 ZonedDateTime 转换为时间戳

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-zoneddatetime-to-timestamp/>

Java 示例将`java.time.ZonedDateTime`转换为`java.sql.Timestamp`，反之亦然。

## 1.ZonedDateTime ->时间戳

TimeExample1.java

```java
 package com.mkyong.jdbc;

import java.sql.Timestamp;
import java.time.ZonedDateTime;

public class TimeExample1 {

    public static void main(String[] args) {

        ZonedDateTime now = ZonedDateTime.now();

        // 1\. ZonedDateTime to TimeStamp
        Timestamp timestamp = Timestamp.valueOf(now.toLocalDateTime());

        // 2\. ZonedDateTime to TimeStamp , no different
        Timestamp timestamp2 = Timestamp.from(now.toInstant());

        System.out.println(now);		// 2019-06-19T14:12:13.585294800+08:00[Asia/Kuala_Lumpur]

        System.out.println(timestamp);	// 2019-06-19 14:12:13.5852948

        System.out.println(timestamp2);	// 2019-06-19 14:12:13.5852948

    }
} 
```

输出

```java
 2019-06-19T14:12:13.585294800+08:00[Asia/Kuala_Lumpur]
2019-06-19 14:12:13.5852948
2019-06-19 14:12:13.5852948 
```

## 2.时间戳->时区日期时间

TimeExample2.java

```java
 package com.mkyong;

import java.sql.Timestamp;
import java.time.Instant;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class TimeExample2 {

    public static void main(String[] args) {

        Timestamp timestamp = Timestamp.from(Instant.now());

        LocalDateTime localDateTimeNoTimeZone = timestamp.toLocalDateTime();

        ZonedDateTime zonedDateTime1 = localDateTimeNoTimeZone.atZone(ZoneId.of("+08:00"));

        ZonedDateTime zonedDateTime2 = localDateTimeNoTimeZone.atZone(ZoneId.of("Asia/Kuala_Lumpur"));

        ZonedDateTime zonedDateTime3 = localDateTimeNoTimeZone.atZone(ZoneId.systemDefault());

        ZonedDateTime zonedDateTime4 = localDateTimeNoTimeZone.atZone(ZoneId.of("-08:00"));

        System.out.println(timestamp);      // 2019-06-19 14:08:23.4458984

        System.out.println(zonedDateTime1); // 2019-06-19T14:08:23.445898400+08:00

        System.out.println(zonedDateTime2); // 2019-06-19T14:08:23.445898400+08:00[Asia/Kuala_Lumpur]

        System.out.println(zonedDateTime3); // 2019-06-19T14:08:23.445898400+08:00[Asia/Kuala_Lumpur]

        System.out.println(zonedDateTime4); // 2019-06-19T14:08:23.445898400-08:00

    }
} 
```

输出

```java
 2019-06-19 14:08:23.4458984
2019-06-19T14:08:23.445898400+08:00
2019-06-19T14:08:23.445898400+08:00[Asia/Kuala_Lumpur]
2019-06-19T14:08:23.445898400+08:00[Asia/Kuala_Lumpur]
2019-06-19T14:08:23.445898400-08:00 
```

# 参考

*   [Java 8–将本地日期时间转换为时间戳](http://web.archive.org/web/20221225035539/https://www.mkyong.com/java8/java-8-convert-localdatetime-to-timestamp/)
*   [ZonedDateTime JavaDocs](http://web.archive.org/web/20221225035539/https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html)
*   [LocalDateTime JavaDocs](http://web.archive.org/web/20221225035539/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)
*   [时间戳 JavaDocs](http://web.archive.org/web/20221225035539/https://docs.oracle.com/javase/8/docs/api/java/sql/Timestamp.html)

<input type="hidden" id="mkyong-current-postId" value="15129">