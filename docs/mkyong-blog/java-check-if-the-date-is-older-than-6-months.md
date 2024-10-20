# Java–检查日期是否超过 30 天或 6 个月

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-check-if-the-date-is-older-than-6-months/>

本文展示了 Java 8 和遗留日期时间 API 示例，以检查日期是否比当前日期早 30 天或 6 个月。

*   [1。Java 8 isBefore()](#java-8-isbefore)
*   [2。Java 8 计时单位。{UNIT}。()之间](#java-8-chronounitunitbetween)
*   [3。Java 8 Period.between()](#java-8-periodbetween)
*   [4。传统日历和日期](#legacy-calendar-and-date)
*   [5。参考文献](#references)

## 1。Java 8 isBefore()

首先减去当前日期，然后使用`isBefore()`检查某个日期是否比当前日期早一段时间。

1.1 此示例检查一个日期是否比当前日期早 6 个月。

JavaMinusMonth.java

```java
 package com.mkyong.app;

import java.time.LocalDate;

public class JavaMinusMonths {

  public static void main(String[] args) {

      LocalDate currentDate = LocalDate.now();
      LocalDate currentDateMinus6Months = currentDate.minusMonths(6);

      // 2021-03-26
      System.out.println("currentDate: " + currentDate);

      // 2020-09-26
      System.out.println("currentDateMinus6Months : " + currentDateMinus6Months);

      LocalDate date1 = LocalDate.of(2019, 8, 23);
      System.out.println("\ndate1 : " + date1);
      if (date1.isBefore(currentDateMinus6Months)) {
          System.out.println("6 months older than current date!");
      }

  }
} 
```

输出

Terminal

```java
 currentDate: 2021-03-26
currentDateMinus6Months : 2020-09-26

date1 : 2019-08-23
6 months older than current date! 
```

1.2 此示例检查一个日期是否比当前日期早 30 天。

JavaMinusDays.java

```java
 package com.mkyong.app;

import java.time.LocalDate;

public class JavaMinusDays {

  public static void main(String[] args) {

      LocalDate currentDate = LocalDate.now();
      LocalDate currentDateMinus30Days = currentDate.minusDays(30);

      // 2021-03-26
      System.out.println("currentDate: " + currentDate);

      // 2020-09-26
      System.out.println("currentDateMinus30Days : " + currentDateMinus30Days);

      LocalDate date1 = LocalDate.of(2019, 8, 23);
      System.out.println("\ndate1 : " + date1);
      if (date1.isBefore(currentDateMinus30Days)) {
          System.out.println("30 months older than current date!");
      }

  }
} 
```

## 2。Java 8 计时单位。{UNIT}。()之间

大多数新的 Java 8 日期和时间 API 都是通过`Temporal`接口实现的，例如`LocalDate`、`LocalDateTime`、`ZonedDateTime`等。我们可以使用`ChronoUnit.{UNIT}.between`来检查两个日期或`Temporal`对象之间的差异。

2.1 审查以下`ChronoUnit.{UNIT}.between`方法:

```java
 ChronoUnit.NANOS.between(Temporal t1, Temporal t2)
ChronoUnit.MICROS.between(Temporal t1, Temporal t2)
ChronoUnit.MILLIS.between(Temporal t1, Temporal t2)
ChronoUnit.SECONDS.between(Temporal t1, Temporal t2)
ChronoUnit.MINUTES.between(Temporal t1, Temporal t2)
ChronoUnit.HOURS.between(Temporal t1, Temporal t2)
ChronoUnit.HALF_DAYS.between(Temporal t1, Temporal t2)
ChronoUnit.DAYS.between(Temporal t1, Temporal t2)
ChronoUnit.WEEKS.between(Temporal t1, Temporal t2)
ChronoUnit.MONTHS.between(Temporal t1, Temporal t2)
ChronoUnit.YEARS.between(Temporal t1, Temporal t2)
ChronoUnit.DECADES.between(Temporal t1, Temporal t2)
ChronoUnit.CENTURIES.between(Temporal t1, Temporal t2)
ChronoUnit.MILLENNIA.between(Temporal t1, Temporal t2)
ChronoUnit.ERAS.between(Temporal t1, Temporal t2) 
```

2.2 这个例子使用`ChronoUnit.MONTHS.between`来检查一个日期是否比当前日期早 6 个月。

JavaChronoUnit1.java

```java
 package com.mkyong.app;

import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

public class JavaChronoUnit1 {

  public static void main(String[] args) {

      LocalDate now = LocalDate.now();    // 2021-03-26
      System.out.println("now: " + now);

      LocalDate date1 = LocalDate.of(2019, 9, 25);

      long months = ChronoUnit.MONTHS.between(now, date1);
      System.out.println(date1);
      System.out.println(months);

      if (months <= -6) {
          System.out.println("6 months older than current date!");
      }

      LocalDate date2 = LocalDate.of(2020, 9, 26);
      long months2 = ChronoUnit.MONTHS.between(now, date2);
      System.out.println(date2);
      System.out.println(months2);
      if (months2 <= -6) {
          System.out.println("6 months older than current date!");
      }

  }
} 
```

输出

Terminal

```java
 now: 2021-03-26

2019-09-25
-18
6 months older than current date!

2020-09-26
-6
6 months older than current date! 
```

2.3 这个例子使用`ChronoUnit.DAYS.between`来检查一个日期是否比当前日期早 30 天。

JavaChronoUnit2.java

```java
 package com.mkyong.app;

import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

public class JavaChronoUnit2 {

  public static void main(String[] args) {

      LocalDate now = LocalDate.now();    // 2021-03-26
      System.out.println("now: " + now);

      LocalDate date1 = LocalDate.of(2019, 9, 25);

      long day1 = ChronoUnit.DAYS.between(now, date1);
      System.out.println(date1);
      System.out.println(day1);

      if (day1 <= -30) {
          System.out.println("30 days older than current date!");
      }

      LocalDate date2 = LocalDate.of(2021, 2, 25);
      long day2 = ChronoUnit.DAYS.between(now, date2);
      System.out.println(date2);
      System.out.println(day2);
      if (day2 <= -30) {
          System.out.println("30 days older than current date!");
      }

  }
} 
```

输出

Terminal

```java
 now: 2021-03-26

2019-09-25
-548
30 days older than current date!

2021-02-25
-29 
```

## 3。Java 8 Period.between()

3.1 阅读一个简单的`java.time.Period`例子，了解周期是如何工作的。

JavaPeriodExample.java

```java
 package com.mkyong.app;

import java.time.LocalDate;
import java.time.Period;

public class JavaPeriodExample {

  public static void main(String[] args) {

      LocalDate date1 = LocalDate.of(2020, 2, 24);
      LocalDate date2 = LocalDate.of(2019, 8, 23);

      System.out.println(date1);  // 2020-02-24
      System.out.println(date2);  // 2019-08-23

      Period period = Period.between(date1, date2);
      String result = String.format("%d years, %d months, %d days",
              period.getYears(), period.getMonths(), period.getDays());

      System.out.println(result); // 0 years, -6 months, -1 days

      LocalDate date3 = LocalDate.of(2019, 1, 1);
      LocalDate date4 = LocalDate.of(2021, 3, 23);

      System.out.println(date3);  // 2019-01-01
      System.out.println(date4);  // 2021-03-23

      Period period2 = Period.between(date3, date4);
      String result2 = String.format("%d years, %d months, %d days",
              period2.getYears(), period2.getMonths(), period2.getDays());

      System.out.println(result2); // 2 years, 2 months, 22 days

  }

} 
```

输出

Terminal

```java
 2020-02-24
2019-08-23
0 years, -6 months, -1 days
2019-01-01
2021-03-23
2 years, 2 months, 22 days 
```

3.2 此示例使用`Period`来检查`LocalDate`是否比当前日期大 6 个月。

JavaPeriodExample2.java

```java
 package com.mkyong.app;

import java.time.LocalDate;
import java.time.Period;

public class JavaPeriodExample2 {

  public static void main(String[] args) {

      LocalDate date1 = LocalDate.of(2020, 9, 25);
      System.out.println("Is 6 months older? " + isOlderThanMonths(date1, 6));

      LocalDate date2 = LocalDate.of(2020, 9, 26);
      System.out.println("Is 6 months older? " + isOlderThanMonths(date2, 6));

      LocalDate date3 = LocalDate.of(2020, 10, 26);
      System.out.println("Is 6 months older? " + isOlderThanMonths(date3, 6));

      LocalDate date4 = LocalDate.of(2001, 10, 26);
      System.out.println("Is 6 months older? " + isOlderThanMonths(date4, 6));

  }

  static boolean isOlderThanMonths(final LocalDate date, final int months) {

      boolean result = false;

      LocalDate now = LocalDate.now();
      // period from now to date
      Period period = Period.between(now, date);

      System.out.println("\nNow: " + now);
      System.out.println("Date: " + date);
      System.out.printf("%d years, %d months, %d days%n",
              period.getYears(), period.getMonths(), period.getDays());

      if (period.getYears() < 0) {
          // if year is negative, 100% older than 6 months
          result = true;
      } else if (period.getYears() == 0) {
          if (period.getMonths() <= -months) {
              result = true;
          }
      }

      return result;

  }

} 
```

输出

Terminal

```java
 Now: 2021-03-26

Date: 2020-09-25
0 years, -6 months, -1 days
Is 6 months older? true

Now: 2021-03-26
Date: 2020-09-26
0 years, -6 months, 0 days
Is 6 months older? true

Now: 2021-03-26
Date: 2020-10-26
0 years, -5 months, 0 days
Is 6 months older? false

Now: 2021-03-26
Date: 2001-10-26
-19 years, -5 months, 0 days
Is 6 months older? true 
```

## 4。传统日历和日期

4.1 本例检查`java.util.Calendar`是否比当前日期早 6 个月。

JavaCalendarExample.java

```java
 package com.mkyong.app;

import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.GregorianCalendar;

public class JavaCalendarExample {

  private static SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");

  public static void main(String[] args) {

      Calendar sixMonthsAgo = Calendar.getInstance();
      // Current date
      // 2021-03-26
      System.out.println("now: " + sdf.format(sixMonthsAgo.getTime()));

      // old way to minus 6 months
      // 2020-09-26
      sixMonthsAgo.add(Calendar.MONTH, -6);
      System.out.println("sixMonthsAgo: " + sdf.format(sixMonthsAgo.getTime()));

      // 2019-06-10
      Calendar date1 = new GregorianCalendar(2020, Calendar.AUGUST, 10);
      System.out.println("date1: " + sdf.format(date1.getTime()));

      if (date1.before(sixMonthsAgo)) {
          System.out.println("6 months older than current date!");
      }

  }

} 
```

输出

Terminal

```java
 now: 2021-03-26
sixMonthsAgo: 2020-09-26
date1: 2020-08-10
6 months older than current date! 
```

4.2 本例检查`java.util.Date`是否比当前日期早 30 天。

JavaDateExample.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

public class JavaDateExample {

    private static SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");

    public static void main(String[] args) throws ParseException {

        Date today = new Date();
        System.out.println("now : " + sdf.format(today));         // 2021-03-26

        Calendar thirtyDaysAgo = Calendar.getInstance();
        thirtyDaysAgo.add(Calendar.DAY_OF_MONTH, -30);            // 2021-02-24

        // convert Calendar to Date
        Date thirtyDaysAgoDate = thirtyDaysAgo.getTime();
        System.out.println("thirtyDaysAgo : " + sdf.format(thirtyDaysAgoDate));

        Date date1 = sdf.parse("2019-12-31");
        if (date1.before(thirtyDaysAgoDate)) {
            System.out.println("30 days older than current date!");
        }

    }

} 
```

输出

Terminal

```java
 now : 2021-03-26
thirtyDaysAgo : 2021-02-24
30 days older than current date! 
```

## 5。参考文献

*   [Java 日期和日历示例](http://web.archive.org/web/20211207220350/https://mkyong.com/java/java-date-and-calendar-examples/)
*   [如何在 Java 中比较日期](http://web.archive.org/web/20211207220350/https://mkyong.com/java/how-to-compare-dates-in-java/)
*   [如何在 Java 中计算货币值](http://web.archive.org/web/20211207220350/https://mkyong.com/java/how-do-calculate-monetary-values-in-java-double-vs-bigdecimal/)
*   [Java–如何获取当前日期时间](http://web.archive.org/web/20211207220350/https://mkyong.com/java/java-how-to-get-current-date-time-date-and-calender/)
*   [局部日期](http://web.archive.org/web/20211207220350/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalDate.html)
*   [局部日期时间](http://web.archive.org/web/20211207220350/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalDateTime.html)
*   [ZonedDateTime](http://web.archive.org/web/20211207220350/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/ZonedDateTime.html)
*   [周期](http://web.archive.org/web/20211207220350/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/Period.html)
*   [计时单位](http://web.archive.org/web/20211207220350/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/temporal/ChronoUnit.html)

<input type="hidden" id="mkyong-current-postId" value="15483">