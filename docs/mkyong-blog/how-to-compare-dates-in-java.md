# 如何在 Java 中比较日期

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-compare-dates-in-java/>

本文展示了几个用 Java 比较两个日期的例子。用 Java 8 例子更新。

*   [1。比较两个日期](#compare-two-date)
    *   [1.1 日期比较](#datecompareto)
    *   [1.2 Date.before()，Date.after()和 Date.equals()](#datebefore-dateafter-and-dateequals)
    *   [1.3 检查日期是否在一定范围内](#check-if-a-date-is-within-a-certain-range)
*   [2。比较两个日历](#compare-two-calendar)
*   [3。比较两个日期和时间(Java 8)](#compare-two-date-and-time-java-8)
    *   [3.1 比较两个本地日期](#compare-two-localdate)
    *   [3.2 比较两个本地日期时间](#compare-two-localdatetime)
    *   [3.3 比较两个瞬间](#compare-two-instant)
*   [4 检查日期是否在一定范围内(Java 8)](#check-if-a-date-is-within-a-certain-range-java-8)
*   [5 检查日期是否超过 6 个月](#check-if-the-date-is-older-than-6-months)
*   [参考文献](#references)

## 1。比较两个日期

对于遗产`java.util.Date`，我们可以用`compareTo`、`before()`、`after()`、`equals()`来比较两个日期。

### 1.1 日期比较

下面的例子使用`Date.compareTo`来比较 Java 中的两个`java.util.Date`。

*   如果参数日期等于`Date`，返回值 0。
*   如果`Date`在参数日期之后，返回值大于 0 或为正。
*   如果`Date`在参数日期之前，返回值小于 0 或负数。

查看`Date.compareTo`方法签名。

Date.java

```java
 package java.util;

public class Date
  implements java.io.Serializable, Cloneable, Comparable<Date> {

  public int compareTo(Date anotherDate) {
      long thisTime = getMillisOf(this);
      long anotherTime = getMillisOf(anotherDate);
      return (thisTime<anotherTime ? -1 : (thisTime==anotherTime ? 0 : 1));
  }

  //...
} 
```

例如:

CompareDate1.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class CompareDate1 {

    public static void main(String[] args) throws ParseException {

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date date1 = sdf.parse("2020-01-30");
        Date date2 = sdf.parse("2020-01-31");

        System.out.println("date1 : " + sdf.format(date1));
        System.out.println("date2 : " + sdf.format(date2));

        int result = date1.compareTo(date2);
        System.out.println("result: " + result);

        if (result == 0) {
            System.out.println("Date1 is equal to Date2");
        } else if (result > 0) {
            System.out.println("Date1 is after Date2");
        } else if (result < 0) {
            System.out.println("Date1 is before Date2");
        } else {
            System.out.println("How to get here?");
        }

    }
} 
```

输出

Terminal

```java
 date1 : 2020-01-30
date2 : 2020-01-31
result: -1
Date1 is before Date2 
```

将`date1`改为`2020-01-31`。

输出

Terminal

```java
 date1 : 2020-01-31
date2 : 2020-01-31
result: 0
Date1 is equal to Date2 
```

将`date1`改为`2020-02-01`。

输出

Terminal

```java
 date1 : 2020-02-01
date2 : 2020-01-31
result: 1
Date1 is after Date2 
```

### 1.2 Date.before()，Date.after()和 Date.equals()

下面是一个更加用户友好的方法来比较 Java 中的两个`java.util.Date`。

CompareDate2.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class CompareDate2 {

  public static void main(String[] args) throws ParseException {

      SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
      //Date date1 = sdf.parse("2009-12-31");
      Date date1 = sdf.parse("2020-02-01");
      Date date2 = sdf.parse("2020-01-31");

      System.out.println("date1 : " + sdf.format(date1));
      System.out.println("date2 : " + sdf.format(date2));

      if (date1.equals(date2)) {
          System.out.println("Date1 is equal Date2");
      }

      if (date1.after(date2)) {
          System.out.println("Date1 is after Date2");
      }

      if (date1.before(date2)) {
          System.out.println("Date1 is before Date2");
      }

  }
} 
```

输出

Terminal

```java
 date1 : 2020-02-01
date2 : 2020-01-31
Date1 is after Date2 
```

### 1.3 检查日期是否在一定范围内

下面的例子使用`getTime()`来检查一个日期是否在两个日期的特定范围内(包括开始日期和结束日期)。

DateRangeValidator.java

```java
 package com.mkyong.app;

import java.util.Date;

public class DateRangeValidator {

    private final Date startDate;
    private final Date endDate;

    public DateRangeValidator(Date startDate, Date endDate) {
        this.startDate = startDate;
        this.endDate = endDate;
    }

    // inclusive startDate and endDate
    // the equals ensure the inclusive of startDate and endDate,
    // if prefer exclusive, just delete the equals
    public boolean isWithinRange(Date testDate) {

        // it works, alternatives
        /*boolean result = false;
        if ((testDate.equals(startDate) || testDate.equals(endDate)) ||
                (testDate.after(startDate) && testDate.before(endDate))) {
            result = true;
        }
        return result;*/

        // compare date and time, inclusive of startDate and endDate
        // getTime() returns number of milliseconds since January 1, 1970, 00:00:00 GMT
        return testDate.getTime() >= startDate.getTime() &&
                testDate.getTime() <= endDate.getTime();
    }

} 
```

DateWithinRange.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateWithinRange {

  public static void main(String[] args) throws ParseException {

      SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");

      Date startDate = sdf.parse("2020-01-01");
      Date endDate = sdf.parse("2020-01-31");

      System.out.println("startDate : " + sdf.format(startDate));
      System.out.println("endDate : " + sdf.format(endDate));

      DateRangeValidator checker = new DateRangeValidator(startDate, endDate);

      Date testDate = sdf.parse("2020-01-01");
      System.out.println("testDate : " + sdf.format(testDate));

      if(checker.isWithinRange(testDate)){
          System.out.println("testDate is within the date range.");
      }else{
          System.out.println("testDate is NOT within the date range.");
      }

  }

} 
```

输出

Terminal

```java
 startDate : 2020-01-01
endDate   : 2020-01-31

testDate  : 2020-01-01
testDate is within the date range. 
```

如果我们把`testDate`改成`2020-03-01`。

输出

Terminal

```java
 startDate : 2020-01-01
endDate   : 2020-01-31

testDate  : 2020-03-01
testDate is NOT within the date range. 
```

## 2。比较两个日历

对于遗产`java.util.Calendar`，`Calendar`的工作方式与`java.util.Date`相同。而`Calendar`包含了类似的`compareTo`、`before()`、`after()`和`equals()`来比较两个日历。

CompareCalendar.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

public class CompareCalendar {

  public static void main(String[] args) throws ParseException {

      SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
      Date date1 = sdf.parse("2020-02-01");
      Date date2 = sdf.parse("2020-01-31");

      Calendar cal1 = Calendar.getInstance();
      Calendar cal2 = Calendar.getInstance();
      cal1.setTime(date1);
      cal2.setTime(date2);

      System.out.println("date1 : " + sdf.format(date1));
      System.out.println("date2 : " + sdf.format(date2));

      if (cal1.after(cal2)) {
          System.out.println("Date1 is after Date2");
      }

      if (cal1.before(cal2)) {
          System.out.println("Date1 is before Date2");
      }

      if (cal1.equals(cal2)) {
          System.out.println("Date1 is equal Date2");
      }

  }

} 
```

输出

Terminal

```java
 date1 : 2020-02-01
date2 : 2020-01-31
Date1 is after Date2 
```

## 3。比较两个日期和时间(Java 8)

对于新的 [Java 8 java.time.*](http://web.archive.org/web/20220627214126/https://mkyong.com/tutorials/java-date-time-tutorials/) 类，都包含类似的`compareTo`、`isBefore()`、`isAfter()`和`isEqual()`来比较两个日期，其工作方式相同。

*   `java.time.LocalDate`–无时间、无时区的日期。
*   `java.time.LocalTime`–无日期、无时区的时间。
*   `java.time.LocalDateTime`–日期和时间，无时区。
*   `java.time.ZonedDateTime`–日期和时间，带时区。
*   `java.time.Instant`–自 Unix 纪元时间(UTC 1970 年 1 月 1 日午夜)以来经过的秒数。

### 3.1 比较两个本地日期

下面的例子展示了如何在 Java 中比较两个`LocalDate`。

CompareLocalDate.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class CompareLocalDate {

    public static void main(String[] args) throws ParseException {

        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu-MM-dd");

        LocalDate date1 = LocalDate.parse("2020-02-01", dtf);
        LocalDate date2 = LocalDate.parse("2020-01-31", dtf);

        System.out.println("date1 : " + date1);
        System.out.println("date2 : " + date2);

        if (date1.isEqual(date2)) {
            System.out.println("Date1 is equal Date2");
        }

        if (date1.isBefore(date2)) {
            System.out.println("Date1 is before Date2");
        }

        if (date1.isAfter(date2)) {
            System.out.println("Date1 is after Date2");
        }

        // test compareTo
        if (date1.compareTo(date2) > 0) {
            System.out.println("Date1 is after Date2");
        } else if (date1.compareTo(date2) < 0) {
            System.out.println("Date1 is before Date2");
        } else if (date1.compareTo(date2) == 0) {
            System.out.println("Date1 is equal to Date2");
        } else {
            System.out.println("How to get here?");
        }

    }

} 
```

输出

Terminal

```java
 date1 : 2020-02-01
date2 : 2020-01-31
Date1 is after Date2
Date1 is after Date2 
```

我们将`date1`改为`2020-01-31`。

输出

Terminal

```java
 date1 : 2020-01-31
date2 : 2020-01-31
Date1 is equal Date2
Date1 is equal to Date2 
```

### 3.2 比较两个本地日期时间

下面的例子展示了如何在 Java 中比较两个`LocalDateTime`，其工作方式相同。

CompareLocalDateTime.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class CompareLocalDateTime {

  public static void main(String[] args) throws ParseException {

      DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu-MM-dd HH:mm:ss");

      LocalDateTime date1 = LocalDateTime.parse("2020-01-31 11:44:43", dtf);
      LocalDateTime date2 = LocalDateTime.parse("2020-01-31 11:44:44", dtf);

      System.out.println("date1 : " + date1);
      System.out.println("date2 : " + date2);

      if (date1.isEqual(date2)) {
          System.out.println("Date1 is equal Date2");
      }

      if (date1.isBefore(date2)) {
          System.out.println("Date1 is before Date2");
      }

      if (date1.isAfter(date2)) {
          System.out.println("Date1 is after Date2");
      }

  }

} 
```

输出

Terminal

```java
 date1 : 2020-01-31T11:44:43
date2 : 2020-01-31T11:44:44
Date1 is before Date2 
```

### 3.3 比较两个瞬间

新的 Java 8 `java.time.Instant`，返回自 Unix 纪元时间(UTC 1970 年 1 月 1 日午夜)以来经过的秒数。

CompareInstant.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.time.Instant;
import java.time.LocalDateTime;
import java.time.ZoneOffset;
import java.time.format.DateTimeFormatter;

public class CompareInstant {

    public static void main(String[] args) throws ParseException {

        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu-MM-dd HH:mm:ss");

        LocalDateTime date1 = LocalDateTime.parse("2020-01-31 11:44:44", dtf);
        LocalDateTime date2 = LocalDateTime.parse("2020-01-31 11:44:44", dtf);

        // convert LocalDateTime to Instant
        Instant instant1 = date1.toInstant(ZoneOffset.UTC);
        Instant instant2 = date2.toInstant(ZoneOffset.UTC);

        // compare getEpochSecond
        if (instant1.getEpochSecond() == instant2.getEpochSecond()) {
            System.out.println("instant1 is equal instant2");
        }

        if (instant1.getEpochSecond() < instant2.getEpochSecond()) {
            System.out.println("instant1 is before instant2");
        }

        if (instant1.getEpochSecond() > instant2.getEpochSecond()) {
            System.out.println("instant1 is after instant2");
        }

        // compare with APIs
        if (instant1.equals(instant2)) {
            System.out.println("instant1 is equal instant2");
        }

        if (instant1.isBefore(instant2)) {
            System.out.println("instant1 is before instant2");
        }

        if (instant1.isAfter(instant2)) {
            System.out.println("instant1 is after instant2");
        }

    }

} 
```

输出

Terminal

```java
 instant1 : 2020-01-31T11:44:44Z
instant2 : 2020-01-31T11:44:44Z
instant1 is equal instant2
instant1 is equal instant2 
```

## 4 检查日期是否在一定范围内(Java 8)

我们可以用简单的`isBefore`、`isAfter`、`isEqual`来检查某个日期是否在某个日期范围内；例如，下面的程序检查`LocalDate`是否在 2020 年 1 月。

DateRangeValidator.java

```java
 package com.mkyong.app;

import java.time.LocalDate;

public class DateRangeValidator {

  private final LocalDate startDate;
  private final LocalDate endDate;

  public DateRangeValidator(LocalDate startDate, LocalDate endDate) {
      this.startDate = startDate;
      this.endDate = endDate;
  }

  public boolean isWithinRange(LocalDate testDate) {

      // exclusive startDate and endDate
      //return testDate.isBefore(endDate) && testDate.isAfter(startDate);

      // inclusive startDate and endDate
      return (testDate.isEqual(startDate) || testDate.isEqual(endDate))
              || (testDate.isBefore(endDate) && testDate.isAfter(startDate));

  }

} 
```

LocalDateWithinRange.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class LocalDateWithinRange {

  public static void main(String[] args) throws ParseException {

      DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu-MM-dd");

      LocalDate startDate = LocalDate.parse("2020-01-01", dtf);
      LocalDate endDate = LocalDate.parse("2020-01-31", dtf);

      System.out.println("startDate : " + startDate);
      System.out.println("endDate : " + endDate);

      DateRangeValidator checker = new DateRangeValidator(startDate, endDate);

      LocalDate testDate = LocalDate.parse("2020-01-01", dtf);
      System.out.println("\ntestDate : " + testDate);

      if (checker.isWithinRange(testDate)) {
          System.out.println("testDate is within the date range.");
      } else {
          System.out.println("testDate is NOT within the date range.");
      }

  }

} 
```

输出

Terminal

```java
 startDate : 2020-01-01
endDate : 2020-01-31

testDate : 2020-01-01
testDate is within the date range. 
```

我们可以对其他 Java 8 时间类使用相同的日期范围算法，如`LocalDateTime`、`ZonedDateTime`和`Instant`。

```java
 public boolean isWithinRange(LocalDateTime testDate) {

      // exclusive startDate and endDate
      //return testDate.isBefore(endDate) && testDate.isAfter(startDate);

      // inclusive startDate and endDate
      return (testDate.isEqual(startDate) || testDate.isEqual(endDate))
              || (testDate.isBefore(endDate) && testDate.isAfter(startDate));

  } 
```

## 5 检查日期是否超过 6 个月

下面显示了一个检查日期是否超过 6 个月的示例。

PlusMonthExample.java

```java
 package com.mkyong.app;

import java.text.ParseException;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class PlusMonthExample {

  public static void main(String[] args) throws ParseException {

      DateTimeFormatter dtf = DateTimeFormatter.ofPattern("uuuu-MM-dd");

      LocalDate now = LocalDate.parse("2020-01-01", dtf);
      LocalDate date1 = LocalDate.parse("2020-07-01", dtf);

      System.out.println("now: " + now);
      System.out.println("date1: " + date1);

      // add 6 months
      LocalDate nowPlus6Months = now.plusMonths(6);
      System.out.println("nowPlus6Months: " + nowPlus6Months);

      System.out.println("If date1 older than 6 months?");

      // if want to exclude the 2020-07-01, remove the isEqual
      if (date1.isAfter(nowPlus6Months) || date1.isEqual(nowPlus6Months)) {
          System.out.println("Yes");
      } else {
          System.out.println("No");
      }

  }

} 
```

输出

Terminal

```java
 now: 2020-01-01
date1: 2020-07-01
nowPlus6Months: 2020-07-01
If date1 older than 6 months?
Yes 
```

**注意**
更多例子来[比较或检查一个日期是否超过 30 天或 6 个月](http://web.archive.org/web/20220627214126/https://mkyong.com/java8/java-check-if-the-date-is-older-than-6-months/)。

## 参考文献

*   [本地日期 JavaDoc](http://web.archive.org/web/20220627214126/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html)
*   [本地日期时间 JavaDoc](http://web.archive.org/web/20220627214126/https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)
*   [句号 JavaDoc](http://web.archive.org/web/20220627214126/https://docs.oracle.com/javase/8/docs/api/java/time/Period.html)
*   [Java 8–如何格式化本地日期时间](http://web.archive.org/web/20220627214126/https://mkyong.com/java8/java-8-how-to-format-localdatetime/)
*   [Java 8–周期和持续时间示例](http://web.archive.org/web/20220627214126/https://mkyong.com/java8/java-8-period-and-duration-examples/)
*   [Java 8–两个本地日期或本地日期时间之间的差异](http://web.archive.org/web/20220627214126/https://mkyong.com/java8/java-8-difference-between-two-localdate-or-localdatetime/)
*   [Java–检查日期是否超过 6 个月](http://web.archive.org/web/20220627214126/https://mkyong.com/java8/java-check-if-the-date-is-older-than-6-months/)

<input type="hidden" id="mkyong-current-postId" value="2981">