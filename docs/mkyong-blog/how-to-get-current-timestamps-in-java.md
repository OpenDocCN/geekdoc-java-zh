# 如何在 Java 中获得当前时间戳

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-current-timestamps-in-java/>

本文展示了几个用 Java 获取当前日期时间或时间戳的 Java 示例。(Java 8 更新)。

代码片段

```java
 // 2021-03-24 16:48:05.591
  Timestamp timestamp = new Timestamp(System.currentTimeMillis());

  // 2021-03-24 16:48:05.591
  Date date = new Date();
  Timestamp timestamp2 = new Timestamp(date.getTime());

  // convert Instant to Timestamp
  Timestamp ts = Timestamp.from(Instant.now())

  // convert ZonedDateTime to Instant to Timestamp
  Timestamp ts = Timestamp.from(ZonedDateTime.now().toInstant()));

  // convert Timestamp to Instant
  Instant instant = ts.toInstant(); 
```

目录

*   [1。Java 时间戳示例](#java-timestamp-examples)
*   [2。将时间转换为时间戳/从时间戳转换为时间](#convert-instant-tofrom-timestamp)
*   [3。将时间戳插入表中](#insert-timestamp-into-a-table)
*   [4。参考文献](#references)

## 1。Java 时间戳示例

下面的程序使用`java.sql.Timestamp`获取当前时间戳，并用`SimpleDateFormat`格式化显示。

TimeStampExample.java

```java
 package com.mkyong.app;

import java.sql.Timestamp;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TimeStampExample {

    // 2021.03.24.16.34.26
    private static final SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy.MM.dd.HH.mm.ss");

    // 2021-03-24T16:44:39.083+08:00
    private static final SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSSXXX");

    // 2021-03-24 16:48:05
    private static final SimpleDateFormat sdf3 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

    public static void main(String[] args) {

        // method 1
        Timestamp timestamp = new Timestamp(System.currentTimeMillis());
        System.out.println(timestamp);                      // 2021-03-24 16:34:26.666

        // method 2 - via Date
        Date date = new Date();
        System.out.println(new Timestamp(date.getTime()));  // 2021-03-24 16:34:26.666
                                                            // number of milliseconds since January 1, 1970, 00:00:00 GMT
        System.out.println(timestamp.getTime());            // 1616574866666

        System.out.println(sdf1.format(timestamp));         // 2021.03.24.16.34.26

        System.out.println(sdf2.format(timestamp));         // 2021-03-24T16:48:05.591+08:00

        System.out.println(sdf3.format(timestamp));         // 2021-03-24 16:48:05

    }
} 
```

输出

Terminal

```java
 2021-03-24 16:48:05.591
2021-03-24 16:48:05.591
1616575685591
2021.03.24.16.48.05
2021-03-24T16:48:05.591+08:00
2021-03-24 16:48:05 
```

## 2。将时间转换为时间戳/从时间戳转换为时间

这个例子展示了如何在新的 Java 8 `java.time.Instant`和旧的`java.sql.Timestamp`之间进行转换。

```java
 // convert Instant to Timestamp
  Timestamp ts = Timestamp.from(Instant.now())

  // convert Timestamp to Instant
  Instant instant = ts.toInstant(); 
```

InstantExample.java

```java
 package com.mkyong.app;

import java.sql.Timestamp;
import java.time.Instant;

public class InstantExample {

  public static void main(String[] args) {

      Timestamp timestamp = new Timestamp(System.currentTimeMillis());
      System.out.println(timestamp);                  // 2021-03-24 17:12:03.311
      System.out.println(timestamp.getTime());        // 1616577123311

      // Convert Timestamp to Instant
      Instant instant = timestamp.toInstant();
      System.out.println(instant);                    // 2021-03-24T09:12:03.311Z
      System.out.println(instant.toEpochMilli());     // 1616577123311

      // Convert Instant to Timestamp
      Timestamp tsFromInstant = Timestamp.from(instant);
      System.out.println(tsFromInstant.getTime());    // 1616577123311
  }
} 
```

输出

Terminal

```java
 2021-03-24 17:12:03.311
1616577123311
2021-03-24T09:12:03.311Z
1616577123311
1616577123311 
```

## 3。将时间戳插入表中

`java.sql.Timestamp`在 JDBC 编程中仍被广泛使用。请参见下面的转换:

```java
 // Java 8, java.time.*

  // convert LocalDateTime to Timestamp
  preparedStatement.setTimestamp(1, Timestamp.valueOf(LocalDateTime.now()));

  // convert Instant to Timestamp
  preparedStatement.setTimestamp(1, Timestamp.from(Instant.now()));

  // Convert ZonedDateTime to Instant to Timestamp
  preparedStatement.setTimestamp(3, Timestamp.from(ZonedDateTime.now().toInstant())); 
```

下面的例子是在表格中插入一个`Timestamp`的 JDBC 例子。

JdbcExample.java

```java
 package com.mkyong.app;

import java.math.BigDecimal;
import java.sql.*;
import java.time.LocalDateTime;

public class JdbcExample {

  private static final String SQL_INSERT = "INSERT INTO EMPLOYEE (NAME, SALARY, CREATED_DATE) VALUES (?,?,?)";

  public static void main(String[] args) {

      try (Connection conn = DriverManager.getConnection(
              "jdbc:postgresql://127.0.0.1:5432/test", "postgres", "password");
           PreparedStatement preparedStatement = conn.prepareStatement(SQL_INSERT)) {

          preparedStatement.setString(1, "mkyong");
          preparedStatement.setBigDecimal(2, new BigDecimal("799.88"));
          preparedStatement.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));

          // preparedStatement.setTimestamp(3, Timestamp.from(ZonedDateTime.now().toInstant()));
          // preparedStatement.setTimestamp(3, Timestamp.from(Instant.now()));

          int row = preparedStatement.executeUpdate();

          // rows affected
          System.out.println(row); //1

      } catch (SQLException e) {
          System.err.format("SQL State: %s\n%s", e.getSQLState(), e.getMessage());
      } catch (Exception e) {
          e.printStackTrace();
      }

  }
} 
```

**注**
更多例子为[用 Java](http://web.archive.org/web/20221229105020/https://mkyong.com/java/java-how-to-get-current-date-time-date-and-calender/) 获取当前日期时间或时间戳。

## 4。参考文献

*   [Java 日期时间教程](http://web.archive.org/web/20221229105020/https://mkyong.com/tutorials/java-date-time-tutorials/)
*   [JDBC 准备报表–插入一行](http://web.archive.org/web/20221229105020/https://mkyong.com/jdbc/jdbc-preparestatement-example-insert-a-record/)
*   [如何在 Java 中获取当前日期时间](http://web.archive.org/web/20221229105020/https://mkyong.com/java/java-how-to-get-current-date-time-date-and-calender/)
*   [维基百科–纪元时间](http://web.archive.org/web/20221229105020/https://en.wikipedia.org/wiki/Unix_time)
*   [维基百科–时间戳](http://web.archive.org/web/20221229105020/https://en.wikipedia.org/wiki/Timestamp)
*   [时间戳](http://web.archive.org/web/20221229105020/https://docs.oracle.com/en/java/javase/11/docs/api/java.sql/java/sql/Timestamp.html)
*   [局部日期时间](http://web.archive.org/web/20221229105020/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/LocalDateTime.html)
*   [ZonedDateTime](http://web.archive.org/web/20221229105020/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/ZonedDateTime.html)
*   [瞬间](http://web.archive.org/web/20221229105020/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/Instant.html)
*   [日期](http://web.archive.org/web/20221229105020/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Date.html)
*   [DateTimeFormatter](http://web.archive.org/web/20221229105020/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/format/DateTimeFormatter.html)
*   [简单日期格式](http://web.archive.org/web/20221229105020/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/SimpleDateFormat.html)

<input type="hidden" id="mkyong-current-postId" value="3760">