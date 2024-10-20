# 如何在 Java 中格式化 FileTime

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-format-filetime-in-java/>

在 Java 中，我们可以使用`DateTimeFormatter`将`FileTime`转换成其他自定义的日期格式。

```java
 public static String formatDateTime(FileTime fileTime) {

        LocalDateTime localDateTime = fileTime
                .toInstant()
                .atZone(ZoneId.systemDefault())
                .toLocalDateTime();

        return localDateTime.format(
              DateTimeFormatter.ofPattern("MM/dd/yyyy HH:mm:ss"));
} 
```

## 1.文件上次修改时间

此示例以自定义日期格式显示文件的上次修改时间。

GetLastModifiedTime.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.BasicFileAttributes;
import java.nio.file.attribute.FileTime;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class GetLastModifiedTime {

    private static final DateTimeFormatter DATE_FORMATTER =
            DateTimeFormatter.ofPattern("MM/dd/yyyy HH:mm:ss");

    public static void main(String[] args) {

        String fileName = "/home/mkyong/test";

        try {

            Path file = Paths.get(fileName);
            BasicFileAttributes attr =
                    Files.readAttributes(file, BasicFileAttributes.class);

            // default YYYY-MM-DDThh:mm:ss[.s+]Z
            System.out.println("lastModifiedTime: " + attr.lastModifiedTime());

            FileTime fileTime = attr.lastModifiedTime();
            System.out.println("lastModifiedTime: " + formatDateTime(fileTime));

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static String formatDateTime(FileTime fileTime) {

        LocalDateTime localDateTime = fileTime
                .toInstant()
                .atZone(ZoneId.systemDefault())
                .toLocalDateTime();

        return localDateTime.format(DATE_FORMATTER);
    }

} 
```

输出

```java
 lastModifiedTime: 2020-07-20T09:29:54.627222Z

lastModifiedTime: 07/20/2020 17:29:54 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [管理文件元数据](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)
*   [Java 日期时间教程](/web/20220619003336/https://mkyong.com/tutorials/java-date-time-tutorials/)
*   [如何在 Java 中获取文件的最后修改日期](/web/20220619003336/https://mkyong.com/java/how-to-get-the-file-last-modified-date-in-java/)

<input type="hidden" id="mkyong-current-postId" value="16033">