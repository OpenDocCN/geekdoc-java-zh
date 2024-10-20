# Java 8–如何计算两个日期之间的天数？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-calculate-days-between-two-dates/>

在 Java 8 中，我们可以使用`ChronoUnit.DAYS.between(from, to)`来计算两个日期之间的天数。

## 1.局部日期

JavaBetweenDays1.java

```java
 package com.mkyong.java8;

import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

public class JavaBetweenDays1 {

    public static void main(String[] args) {

        LocalDate from = LocalDate.now();
        LocalDate to = from.plusDays(10);

        long result = ChronoUnit.DAYS.between(from, to);
        System.out.println(result);    // 10

    }

} 
```

输出

```java
 10 
```

## 2.本地日期时间

JavaBetweenDays2.java

```java
 package com.mkyong.java8;

import java.time.LocalDateTime;
import java.time.temporal.ChronoUnit;

public class JavaBetweenDays2 {

    public static void main(String[] args) {

        LocalDateTime from = LocalDateTime.now();
        LocalDateTime to = from.plusDays(10);

        long result = ChronoUnit.DAYS.between(from, to);
        System.out.println(result);     // 10

        LocalDateTime to2 = from.minusDays(10);
        long result2 = ChronoUnit.DAYS.between(from, to2);
        System.out.println(result2);    // -10

    }

} 
```

输出

```java
 10
-10 
```

## 参考

*   [Oracle–Java SE 8 日期和时间](http://web.archive.org/web/20221207074331/https://www.oracle.com/technical-resources/articles/java/jf14-date-time.html)
*   [Java 日期时间教程](/web/20221207074331/https://mkyong.com/tutorials/java-date-time-tutorials/)
*   [Java 8–两个本地日期或本地日期时间之间的差异](/web/20221207074331/https://mkyong.com/java8/java-8-difference-between-two-localdate-or-localdatetime/)

<input type="hidden" id="mkyong-current-postId" value="15410">