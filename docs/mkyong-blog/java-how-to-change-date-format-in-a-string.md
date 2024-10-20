# Java——如何改变字符串中的日期格式

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-change-date-format-in-a-string/>

如果 Java 8，`DateTimeFormatter`，否则`SimpleDateFormat`改变字符串中的日期格式。

## 1\. DateTimeFormatter (Java 8)

将字符串转换为`LocalDateTime`并用`DateTimeFormatter`改变日期格式

DateFormatExample1.java

```java
 package com.mkyong;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class DateFormatExample1 {

	// date format 1
    private static final DateTimeFormatter dateFormatter 
		= DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss.S");

	// date format 2
    private static final DateTimeFormatter dateFormatterNew 
		= DateTimeFormatter.ofPattern("EEEE, MMM d, yyyy HH:mm:ss a");

    public static void main(String[] args) {

        String date = "2019-05-23 00:00:00.0";

		// string to LocalDateTime
        LocalDateTime ldateTime = LocalDateTime.parse(date, dateFormatter);

        System.out.println(dateFormatter.format(ldateTime));

        // change date format
        System.out.println(dateFormatterNew.format(ldateTime));

    }

} 
```

输出

```java
 2019-05-23 00:00:00.0

Thursday, May 23, 2019 00:00:00 AM 
```

**Note**
More Java 8 [String to LocalDateTime examples](http://web.archive.org/web/20210817223541/https://www.mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)

## 2.简单日期格式

将字符串转换为`Date`并用`SimpleDateFormat`改变日期格式

SimpleDateFormatExample.java

```java
 package com.mkyong;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class SimpleDateFormatExample {

    private static final SimpleDateFormat sdf = 
			new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.S");

    private static final SimpleDateFormat sdfNew = 
			new SimpleDateFormat("EEEE, MMM d, yyyy HH:mm:ss a");

    public static void main(String[] args) {

        String dateString = "2019-05-23 00:00:00.0";

        try {

			// string to date
            Date date = sdf.parse(dateString);

            System.out.println(sdf.format(date));

            System.out.println(sdfNew.format(date));

        } catch (ParseException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 2019-05-23 00:00:00.0

Thursday, May 23, 2019 00:00:00 AM 
```

**Note**
More Java [String to Date examples](http://web.archive.org/web/20210817223541/https://www.mkyong.com/java/how-to-convert-string-to-date-java/)

## 参考

*   [datetime formatter JavaDocs](http://web.archive.org/web/20210817223541/https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)
*   [SimpleDateFormat JavaDocs](http://web.archive.org/web/20210817223541/https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)
*   [Java 8–如何将字符串转换为本地日期](http://web.archive.org/web/20210817223541/https://www.mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)
*   [如何将字符串转换为日期–Java](http://web.archive.org/web/20210817223541/https://www.mkyong.com/java/how-to-convert-string-to-date-java/)

Tags : [date](http://web.archive.org/web/20210817223541/https://mkyong.com/tag/date/) [java](http://web.archive.org/web/20210817223541/https://mkyong.com/tag/java/) [java 8](http://web.archive.org/web/20210817223541/https://mkyong.com/tag/java-8/)<input type="hidden" id="mkyong-current-postId" value="15097">