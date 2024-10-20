# Java 8–两个本地日期或本地日期时间之间的差异

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-difference-between-two-localdate-or-localdatetime/>

在 Java 8 中，我们可以用`Period`、`Duration`或`ChronoUnit`来计算两个`LocalDate`或`LocaldateTime`的差值。

1.  `Period`计算两个`LocalDate`之差。
2.  `Duration`计算两个`LocalDateTime`之差。
3.  `ChronoUnit`为一切。

## 1.时期

JavaLocalDate.java

```java
 package com.mkyong.java8;

import java.time.LocalDate;
import java.time.Period;

public class JavaLocalDate {

    public static void main(String[] args) {

        LocalDate from = LocalDate.of(2020, 5, 4);
        LocalDate to = LocalDate.of(2020, 10, 10);

        Period period = Period.between(from, to);

        System.out.print(period.getYears() + " years,");
        System.out.print(period.getMonths() + " months,");
        System.out.print(period.getDays() + " days");

    }

} 
```

输出

```java
 0 years,5 months,6 days 
```

## 2.持续时间

JavaLocalDateTime.java

```java
 package com.mkyong.java8;

import java.time.Duration;
import java.time.LocalDateTime;

public class JavaLocalDateTime {

    public static void main(String[] args) {

        LocalDateTime from = LocalDateTime.of(2020, 10, 4,
                10, 20, 55);
        LocalDateTime to = LocalDateTime.of(2020, 10, 10,
                10, 21, 1);

        Duration duration = Duration.between(from, to);

        // days between from and to
        System.out.println(duration.toDays() + " days");

        // hours between from and to
        System.out.println(duration.toHours() + " hours");

        // minutes between from and to
        System.out.println(duration.toMinutes() + " minutes");

        // seconds between from and to
        System.out.println(duration.toSeconds() + " seconds");
        System.out.println(duration.getSeconds() + " seconds");

    }

} 
```

输出

```java
 6 days
144 hours
8640 minutes
518406 seconds
518406 seconds 
```

## 3.计时单位

JavaChronoUnit.java

```java
 package com.mkyong.java8;

import java.time.LocalDateTime;
import java.time.temporal.ChronoUnit;

public class JavaChronoUnit {

    public static void main(String[] args) {

        LocalDateTime from = LocalDateTime.of(2020, 10, 4,
                10, 20, 55);
        LocalDateTime to = LocalDateTime.of(2020, 11, 10,
                10, 21, 1);

        long years = ChronoUnit.YEARS.between(from, to);
        long months = ChronoUnit.MONTHS.between(from, to);
        long weeks = ChronoUnit.WEEKS.between(from, to);
        long days = ChronoUnit.DAYS.between(from, to);
        long hours = ChronoUnit.HOURS.between(from, to);
        long minutes = ChronoUnit.MINUTES.between(from, to);
        long seconds = ChronoUnit.SECONDS.between(from, to);
        long milliseconds = ChronoUnit.MILLIS.between(from, to);
        long nano = ChronoUnit.NANOS.between(from, to);

        System.out.println(years + " years");
        System.out.println(months + " months");
        System.out.println(weeks + " weeks");
        System.out.println(days + " days");
        System.out.println(hours + " hours");
        System.out.println(minutes + " minutes");
        System.out.println(seconds + " seconds");
        System.out.println(milliseconds + " milliseconds");
        System.out.println(nano + " nano");

    }

} 
```

输出

```java
 0 years
1 months
5 weeks
37 days
888 hours
53280 minutes
3196806 seconds
3196806000 milliseconds
3196806000000000 nano 
```

## 参考

*   [Java 8–周期和持续时间示例](/web/20221129200847/https://mkyong.com/java8/java-8-period-and-duration-examples/)
*   [如何在 Java 中比较日期](java/how-to-compare-dates-in-java/)

<input type="hidden" id="mkyong-current-postId" value="15408">