# Java 8–将纪元时间毫秒转换为本地日期或本地日期时间

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-epoch-time-milliseconds-to-localdate-or-localdatetime/>

在 Java 8 中，我们可以使用`Instant.ofEpochMilli().atZone()`将以毫秒为单位的纪元时间转换回`LocalDate`或`LocalDateTime`

历元时间至`LocalDate`

```java
 LocalDate ld = Instant.ofEpochMilli(epoch).atZone(ZoneId.systemDefault()).toLocalDate(); 
```

历元时间至`LocalDateTime`

```java
 LocalDateTime ldt = Instant.ofEpochMilli(epoch).atZone(ZoneId.systemDefault()).toLocalDateTime(); 
```

*P . S . Epoch time 是自 1970 年 1 月 1 日世界协调时 0:00:00 起经过的秒数*

## 1.epoch time-> localdate | | localdatetime

获取长值形式的当前历元时间，并将其转换回`LocalDate`或`LocalDateTime`。

Java8ConvertEpoch.java

```java
 package com.mkyong.java8;

import java.time.Instant;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.ZoneId;

public class Java8ConvertEpoch {

    public static void main(String[] args) {

        long epoch = Instant.now().toEpochMilli();
        System.out.println(epoch);

        LocalDate ld = Instant.ofEpochMilli(epoch)
                .atZone(ZoneId.systemDefault()).toLocalDate();
        System.out.println(ld);

        LocalDateTime ldt = Instant.ofEpochMilli(epoch)
                .atZone(ZoneId.systemDefault()).toLocalDateTime();
        System.out.println(ldt);

    }

} 
```

输出

```java
 1581420629955
2020-02-11
2020-02-11T19:30:29.955 
```

## 参考

*   [维基百科–纪元时间](http://web.archive.org/web/20221206104548/https://en.wikipedia.org/wiki/Unix_time)

<input type="hidden" id="mkyong-current-postId" value="15404">