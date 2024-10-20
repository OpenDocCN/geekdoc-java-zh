# Java 8–如何格式化本地日期时间

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-format-localdatetime/>

几个例子向你展示如何在 Java 8 中格式化`java.time.LocalDateTime`。

## 1\. LocalDateTime + DateTimeFormatter

要格式化 LocalDateTime 对象，使用`DateTimeFormatter`

TestDate1.java

```java
 package com.mkyong.time;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class TestDate1 {
    public static void main(String[] args) {

        //Get current date time
        LocalDateTime now = LocalDateTime.now();

        System.out.println("Before : " + now);

        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

        String formatDateTime = now.format(formatter);

        System.out.println("After : " + formatDateTime);

    }
} 
```

输出

```java
 Before : 2016-11-09T11:44:44.797

After  : 2016-11-09 11:44:44 
```

## 2.字符串-> LocalDateTime

另一个将字符串转换为`LocalDateTime`的例子

TestDate2.java

```java
 package com.mkyong.time;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class TestDate2 {
    public static void main(String[] args) {

        String now = "2016-11-09 10:30";

        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");

        LocalDateTime formatDateTime = LocalDateTime.parse(now, formatter);

        System.out.println("Before : " + now);

        System.out.println("After : " + formatDateTime);

        System.out.println("After : " + formatDateTime.format(formatter));

    }
} 
```

输出

```java
 Before : 2016-11-09 10:30

After : 2016-11-09T10:30

After : 2016-11-09 10:30 
```

## 参考

1.  [DateTimeFormatter JavaDoc](http://web.archive.org/web/20221120203614/https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)
2.  [Java 8–如何将字符串转换为本地日期](http://web.archive.org/web/20221120203614/http://www.mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)

<input type="hidden" id="mkyong-current-postId" value="14071">