# Java 8–周期和持续时间示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-period-and-duration-examples/>

几个例子向你展示如何使用 Java 8 `Duration`、`Period`和`ChronoUnit`对象找出日期之间的差异。

1.  持续时间–以秒和纳秒为单位测量时间。
2.  period–以年、月和日为单位测量时间。

## 1.持续时间示例

一个`java.time.Duration`示例，用于找出两个`LocalDateTime`之间的秒数差异

DurationExample.java

```java
 package com.mkyong.time;

import java.time.Duration;
import java.time.LocalDateTime;
import java.time.Month;
import java.time.temporal.ChronoUnit;

public class DurationExample {

    public static void main(String[] args) {

		// Creating Durations
        System.out.println("--- Examples --- ");

        Duration oneHours = Duration.ofHours(1);
        System.out.println(oneHours.getSeconds() + " seconds");

        Duration oneHours2 = Duration.of(1, ChronoUnit.HOURS);
        System.out.println(oneHours2.getSeconds() + " seconds");

		// Test Duration.between
        System.out.println("\n--- Duration.between --- ");

        LocalDateTime oldDate = LocalDateTime.of(2016, Month.AUGUST, 31, 10, 20, 55);
        LocalDateTime newDate = LocalDateTime.of(2016, Month.NOVEMBER, 9, 10, 21, 56);

        System.out.println(oldDate);
        System.out.println(newDate);

        //count seconds between dates
        Duration duration = Duration.between(oldDate, newDate);

        System.out.println(duration.getSeconds() + " seconds");

    }
} 
```

输出

```java
 --- Examples --- 
3600 seconds
3600 seconds

--- Duration.between --- 
2016-08-31T10:20:55
2016-11-09T10:21:56
6048061 seconds 
```

## 2.期间示例

一个`java.time.Period`示例，用于找出两个`LocalDates`之间的不同(年、月、日)

PeriodExample.java

```java
 package com.mkyong.time;

import java.time.LocalDate;
import java.time.Month;
import java.time.Period;

public class PeriodExample {

    public static void main(String[] args) {

        System.out.println("--- Examples --- ");

        Period tenDays = Period.ofDays(10); 
        System.out.println(tenDays.getDays()); //10

        Period oneYearTwoMonthsThreeDays = Period.of(1, 2, 3);
        System.out.println(oneYearTwoMonthsThreeDays.getYears());   //1
        System.out.println(oneYearTwoMonthsThreeDays.getMonths());  //2
        System.out.println(oneYearTwoMonthsThreeDays.getDays());    //3

        System.out.println("\n--- Period.between --- ");
        LocalDate oldDate = LocalDate.of(1982, Month.AUGUST, 31);
        LocalDate newDate = LocalDate.of(2016, Month.NOVEMBER, 9);

        System.out.println(oldDate);
        System.out.println(newDate);

        // check period between dates
        Period period = Period.between(oldDate, newDate);

        System.out.print(period.getYears() + " years,");
        System.out.print(period.getMonths() + " months,");
        System.out.print(period.getDays() + " days");

    }
} 
```

输出

```java
 --- Examples --- 
10
1
2
3

--- Period.between --- 
1982-08-31
2016-11-09
34 years,2 months,9 days 
```

## 3.计时单位示例

或者，您可以使用`ChronoUnit.{unit}.between`找出日期之间的差异，查看以下示例:

ChronoUnitExample.java

```java
 package com.mkyong.time;

import java.time.LocalDateTime;
import java.time.Month;
import java.time.temporal.ChronoUnit;

public class ChronoUnitExample {

    public static void main(String[] args) {

        LocalDateTime oldDate = LocalDateTime.of(1982, Month.AUGUST, 31, 10, 20, 55);
        LocalDateTime newDate = LocalDateTime.of(2016, Month.NOVEMBER, 9, 10, 21, 56);

        System.out.println(oldDate);
        System.out.println(newDate);

        // count between dates
        long years = ChronoUnit.YEARS.between(oldDate, newDate);
        long months = ChronoUnit.MONTHS.between(oldDate, newDate);
        long weeks = ChronoUnit.WEEKS.between(oldDate, newDate);
        long days = ChronoUnit.DAYS.between(oldDate, newDate);
        long hours = ChronoUnit.HOURS.between(oldDate, newDate);
        long minutes = ChronoUnit.MINUTES.between(oldDate, newDate);
        long seconds = ChronoUnit.SECONDS.between(oldDate, newDate);
        long milis = ChronoUnit.MILLIS.between(oldDate, newDate);
        long nano = ChronoUnit.NANOS.between(oldDate, newDate);

        System.out.println("\n--- Total --- ");
        System.out.println(years + " years");
        System.out.println(months + " months");
        System.out.println(weeks + " weeks");
        System.out.println(days + " days");
        System.out.println(hours + " hours");
        System.out.println(minutes + " minutes");
        System.out.println(seconds + " seconds");
        System.out.println(milis + " milis");
        System.out.println(nano + " nano");

    }
} 
```

输出

```java
 1982-08-31T10:20:55
2016-11-09T10:21:56

--- Total --- 
34 years
410 months
1784 weeks
12489 days
299736 hours
17984161 minutes
1079049661 seconds
1079049661000 milis
1079049661000000000 nano 
```

## 参考

1.  [甲骨文教程–周期和持续时间](http://web.archive.org/web/20221027024415/https://docs.oracle.com/javase/tutorial/datetime/iso/period.html)
2.  [持续时间 JavaDoc](http://web.archive.org/web/20221027024415/https://docs.oracle.com/javase/8/docs/api/java/time/Duration.html)
3.  [句号 JavaDoc](http://web.archive.org/web/20221027024415/https://docs.oracle.com/javase/8/docs/api/java/time/Period.html)
4.  [计时单位 JavaDoc](http://web.archive.org/web/20221027024415/https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoUnit.html)

<input type="hidden" id="mkyong-current-postId" value="14072">