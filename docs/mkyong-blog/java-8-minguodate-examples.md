# Java 8–MinguoDate 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-minguodate-examples/>

这种[民国日期](http://web.archive.org/web/20210817080847/https://docs.oracle.com/javase/8/docs/api/java/time/chrono/MinguoDate.html)日历系统主要在台湾(中华民国……)使用

```java
(ISO) 1912-01-01 = 1-01-01 (Minguo ROC)

```

例如，要将当前日期转换为民国日期，只需用数字 1911 减去当前年份

```java
2016 (ISO) - 1911 = 105 (Minguo ROC)

```

## 1.LocalDate -> MinguoDate

查看完整的示例，将`LocalDate`转换为`MinguoDate`

TestMinguoDate.java

```java
 package com.mkyong.date;

import java.time.LocalDate;
import java.time.chrono.MinguoDate;

public class TestMinguoDate {

    public static void main(String[] args) {

        // LocalDate -> MinguoDate
        System.out.println("Example 1...");        

        LocalDate localDate = LocalDate.of(1912, 1, 1);
        MinguoDate minguo = MinguoDate.from(localDate);
        System.out.println("LocalDate : " + localDate); //1912-01-01
        System.out.println("MinguoDate : " + minguo);   //1-01-01

        // MinguoDate -> LocalDate
        System.out.println("\nExample 2...");

        MinguoDate minguo2 = MinguoDate.of(105, 8, 24);
        //LocalDate localDate = LocalDate.ofEpochDay(minguo2.toEpochDay());
        LocalDate localDate2 = LocalDate.from(minguo2);
        System.out.println("MinguoDate : " + minguo2);   //105-08-24
        System.out.println("LocalDate : " + localDate2); //2016-08-24

    }

} 
```

输出

```java
 Example 1...
LocalDate : 1912-01-01
MinguoDate : Minguo ROC 1-01-01

Example 2...
MinguoDate : Minguo ROC 105-08-24
LocalDate : 2016-08-24 
```

## 参考

1.  [MinguoDate JavaDoc](http://web.archive.org/web/20210817080847/https://docs.oracle.com/javase/8/docs/api/java/time/chrono/MinguoDate.html)
2.  [民国历](http://web.archive.org/web/20210817080847/http://calendars.wikia.com/wiki/Minguo_calendar)

Tags : [java.time](http://web.archive.org/web/20210817080847/https://mkyong.com/tag/java-time/) [java8](http://web.archive.org/web/20210817080847/https://mkyong.com/tag/java8/) [minguo](http://web.archive.org/web/20210817080847/https://mkyong.com/tag/minguo/)<input type="hidden" id="mkyong-current-postId" value="14060">