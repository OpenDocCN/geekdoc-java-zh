# Java 8–将即时转换为时区日期时间

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-instant-to-zoneddatetime/>

Java 8 示例向您展示如何从`Instant`转换到`ZonedDateTime`

## 1.即时->时区日期时间

将`Instant` UTC+0 转换为日本`ZonedDateTime` UTC+9 的示例

InstantZonedDateTime1.java

```java
 package com.mkyong.date;

import java.time.Instant;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class InstantZonedDateTime1 {

    public static void main(String[] argv) {

        // Z = UTC+0
        Instant instant = Instant.now();

        System.out.println("Instant : " + instant);

        // Japan = UTC+9
        ZonedDateTime jpTime = instant.atZone(ZoneId.of("Asia/Tokyo"));

        System.out.println("ZonedDateTime : " + jpTime);

        System.out.println("OffSet : " + jpTime.getOffset());

    }

} 
```

输出

```java
 Instant : 2016-08-18T06:17:10.225Z
LocalDateTime : 2016-08-18T06:17:10.225 
```

## 2.ZonedDateTime ->即时

将日本的`ZonedDateTime` UTC+9 转换回一个`Instant` UTC+0/Z，查看结果，自动调整偏移量。

InstantZonedDateTime2.java

```java
 package com.mkyong.date;

import java.time.*;

public class InstantZonedDateTime2 {

    public static void main(String[] argv) {

        LocalDateTime dateTime = LocalDateTime.of(2016, Month.AUGUST, 18, 6, 57, 38);

        // UTC+9
        ZonedDateTime jpTime = dateTime.atZone(ZoneId.of("Asia/Tokyo"));

        System.out.println("ZonedDateTime : " + jpTime);

        // Convert to instant UTC+0/Z , java.time helps to reduce 9 hours
        Instant instant = jpTime.toInstant();

        System.out.println("Instant : " + instant);

    }

} 
```

输出

```java
 ZonedDateTime : 2016-08-18T06:57:38+09:00[Asia/Tokyo]
Instant : 2016-08-17T21:57:38Z 
```

## 参考

1.  [维基百科–ISO 8601 日期格式](http://web.archive.org/web/20220627214124/https://en.wikipedia.org/wiki/ISO_8601)
2.  [即时 JavaDoc](http://web.archive.org/web/20220627214124/https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html)
3.  [ZoneddateTime JavaDoc](http://web.archive.org/web/20220627214124/https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html)

<input type="hidden" id="mkyong-current-postId" value="14045">