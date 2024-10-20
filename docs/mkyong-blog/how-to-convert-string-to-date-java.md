# 如何将字符串转换为日期–Java

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-convert-string-to-date-java/>

在本教程中，我们将向您展示如何将字符串转换为`java.util.Date`。许多 Java 初学者被日期转换所困扰，希望这篇总结指南能在某些方面帮助你。

```java
 // String -> Date
    SimpleDateFormat.parse(String);

    // Date -> String
    SimpleDateFormat.format(date); 
```

关于`java.text.SimpleDateFormat`中使用的一些常见的日期和时间模式，请参考下表 [JavaDoc](http://web.archive.org/web/20221213225441/https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)

| 信 | 描述 | 例子 |
| y | 年 | Two thousand and thirteen |
| M | 一年中的月份 | 2007 年 7 月 |
| d | 一个月中的第几天 | 1-31 |
| E | 周中的日名称 | 星期五，星期天 |
| a | Am/pm 标记 | 上午，下午 |
| H | 一天中的小时 | 0-23 |
| h | 上午/下午的小时 | 1-12 |
| m | 小时中的分钟 | 0-60 |
| s | 分钟秒 | 0-60 |

**Note**
You may interest at this Java 8 example – [How to convert String to LocalDate](http://web.archive.org/web/20221213225441/http://www.mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)

## 1.string = 2013 年 6 月 7 日

如果是 3 'M '，则月份被解释为文本(周一至十二月)，否则为数字(01-12)。

TestDateExample1.java

```java
 package com.mkyong.date;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TestDateExample1 {

    public static void main(String[] argv) {

        SimpleDateFormat formatter = new SimpleDateFormat("dd-MMM-yyyy");
        String dateInString = "7-Jun-2013";

        try {

            Date date = formatter.parse(dateInString);
            System.out.println(date);
            System.out.println(formatter.format(date));

        } catch (ParseException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Fri Jun 07 00:00:00 MYT 2013
07-Jun-2013 
```

## 2.string = 2013 年 7 月 6 日

TestDateExample2.java

```java
 package com.mkyong.date;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TestDateExample2 {

    public static void main(String[] argv) {

        SimpleDateFormat formatter = new SimpleDateFormat("dd/MM/yyyy");
        String dateInString = "07/06/2013";

        try {

            Date date = formatter.parse(dateInString);
            System.out.println(date);
            System.out.println(formatter.format(date));

        } catch (ParseException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Fri Jun 07 00:00:00 MYT 2013
07/06/2013 
```

## 3.String = Fri，2013 年 6 月 7 日

TestDateExample3.java

```java
 package com.mkyong.date;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TestDateExample3 {

    public static void main(String[] argv) {

        SimpleDateFormat formatter = new SimpleDateFormat("E, MMM dd yyyy");
        String dateInString = "Fri, June 7 2013";

        try {

            Date date = formatter.parse(dateInString);
            System.out.println(date);
            System.out.println(formatter.format(date));

        } catch (ParseException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Fri Jun 07 00:00:00 MYT 2013
Fri, Jun 07 2013 
```

## 4.String =年 6 月 7 日星期五下午 12 点 10 分 56 秒

TestDateExample4.java

```java
 package com.mkyong.date;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TestDateExample4 {

    public static void main(String[] argv) {

        SimpleDateFormat formatter = new SimpleDateFormat("EEEE, MMM dd, yyyy HH:mm:ss a");
        String dateInString = "Friday, Jun 7, 2013 12:10:56 PM";

        try {

            Date date = formatter.parse(dateInString);
            System.out.println(date);
            System.out.println(formatter.format(date));

        } catch (ParseException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Fri Jun 07 12:10:56 MYT 2013
Friday, Jun 07, 2013 12:10:56 PM 
```

## 5.String = 2014-10-05T15:23:01Z

Z 后缀表示 UTC，`java.util.SimpleDateFormat`解析不正确，需要用'+0000 '替换后缀 Z。

TestDateExample5.java

```java
 package com.mkyong.date;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TestDateExample5 {

    public static void main(String[] argv) {

        SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String dateInString = "2014-10-05T15:23:01Z";

        try {

            Date date = formatter.parse(dateInString.replaceAll("Z$", "+0000"));
            System.out.println(date);

            System.out.println("time zone : " + TimeZone.getDefault().getID());
            System.out.println(formatter.format(date));

        } catch (ParseException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Sun Oct 05 23:23:01 MYT 2014
time zone : Asia/Kuala_Lumpur
2014-10-05T23:23:01+0800 
```

在 Java 8 中，你可以把它转换成一个`java.time.Instant`对象，用指定的时区显示。

TestDateExample6.java

```java
 package com.mkyong.date;

import java.time.*;

public class TestDateExample6 {

    public static void main(String[] argv) {

        String dateInString = "2014-10-05T15:23:01Z";

        Instant instant = Instant.parse(dateInString);

        System.out.println(instant);

        //get date time only
        LocalDateTime result = LocalDateTime.ofInstant(instant, ZoneId.of(ZoneOffset.UTC.getId()));

        System.out.println(result);

        //get date time + timezone
        ZonedDateTime zonedDateTime = instant.atZone(ZoneId.of("Africa/Tripoli"));
        System.out.println(zonedDateTime);

        //get date time + timezone
        ZonedDateTime zonedDateTime2 = instant.atZone(ZoneId.of("Europe/Athens"));
        System.out.println(zonedDateTime2);

    }

} 
```

输出

```java
 2014-10-05T15:23:01Z
2014-10-05T15:23:01
2014-10-05T17:23:01+02:00[Africa/Tripoli]
2014-10-05T18:23:01+03:00[Europe/Athens] 
```

## 参考

1.  [简单日期格式 JavaDoc](http://web.archive.org/web/20221213225441/https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html)
2.  [Java 8–如何将字符串转换为本地日期](http://web.archive.org/web/20221213225441/http://www.mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)
3.  [stack overflow:simple date format 用“Z”字面值解析日期](http://web.archive.org/web/20221213225441/https://stackoverflow.com/questions/2580925/simpledateformat-parsing-date-with-z-literal)
4.  [维基百科:ISO 8601](http://web.archive.org/web/20221213225441/https://en.wikipedia.org/wiki/ISO_8601)
5.  [时区和时差等级](http://web.archive.org/web/20221213225441/https://docs.oracle.com/javase/tutorial/datetime/iso/timezones.html)
6.  [格林威治时间 VS 世界协调时](http://web.archive.org/web/20221213225441/http://www.diffen.com/difference/GMT_vs_UTC)
7.  什么是时区？
8.  [Joda 时间](http://web.archive.org/web/20221213225441/http://www.joda.org/joda-time/)

<input type="hidden" id="mkyong-current-postId" value="12991">