# Java–更新文件的上次修改日期

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-change-the-file-last-modified-date-in-java/>

在 Java 中，我们可以使用 NIO `Files.setLastModifiedTime(path, FileTime)`来更新文件的最后修改日期或时间。

```java
 Path path = Paths.get("/path/file");

  LocalDate newLocalDate = LocalDate.of(1997, 12, 31);

  // year, month, dayOfMonth, hour, minute, second
  LocalDateTime newLocalDateTime = LocalDateTime.of(1999, 9, 30, 10, 30, 22);

  // convert LocalDateTime to Instant
  Instant instant = newLocalDateTime.toInstant(ZoneOffset.UTC);

  // convert LocalDate to Instant, need a time zone
  Instant instant = newLocalDate.atStartOfDay(ZoneId.systemDefault()).toInstant();

  // convert instant to filetime
  // update last modified time of a file
  Files.setLastModifiedTime(path, FileTime.from(instant)); 
```

## 1。Java NIO——更新文件的上次修改日期。

对于 Java NIO `java.nio.file.Files`，我们可以使用`Files.setLastModifiedTime()`来更新文件的最后修改日期。

UpdateLastModifiedTime.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.BasicFileAttributes;
import java.nio.file.attribute.FileTime;
import java.time.Instant;
import java.time.LocalDate;
import java.time.ZoneId;

public class UpdateLastModifiedTime {

    public static void main(String[] args) throws IOException {

        String fileName = "c:\\test\\google.png";

        Path file = Paths.get(fileName);
        BasicFileAttributes attr = Files.readAttributes(file, BasicFileAttributes.class);
        FileTime lastModifiedTime = attr.lastModifiedTime();

        // print original last modified time
        System.out.println("[original] lastModifiedTime:" + lastModifiedTime);

        LocalDate newLocalDate = LocalDate.of(1998, 9, 30);
        // convert LocalDate to instant, need time zone
        Instant instant = newLocalDate.atStartOfDay(ZoneId.systemDefault()).toInstant();

        // convert instant to filetime
        // update last modified time
        Files.setLastModifiedTime(file, FileTime.from(instant));

        // read last modified time again
        FileTime newLastModifiedTime = Files.readAttributes(file,
                BasicFileAttributes.class).lastModifiedTime();
        System.out.println("[updated] lastModifiedTime:" + newLastModifiedTime);

    }

} 
```

输出

Terminal

```java
 [original] lastModifiedTime:2020-12-01T07:23:48.405691Z
[updated] lastModifiedTime:1998-09-29T16:00:00Z 
```

查看`Files.setLastModifiedTime`方法签名，新的修改时间是一个 [FileTime](http://web.archive.org/web/20220619003340/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/attribute/FileTime.html) 。

Files.java

```java
 package java.nio.file;

  //...
  public static Path setLastModifiedTime(Path path, FileTime time)
       throws IOException
   {
       getFileAttributeView(path, BasicFileAttributeView.class)
           .setTimes(Objects.requireNonNull(time), null, null);
       return path;
   } 
```

我们可以使用`FileTime.from(Instant instant)`将一个 Java 8 `Instant`转换成一个`FileTime`。

```java
 Files.setLastModifiedTime(file, FileTime.from(instant)); 
```

或`FileTime.fromMillis(long value)`将从 epoch 开始的毫秒数转换为`FileTime`。

```java
 FileTime now = FileTime.fromMillis(System.currentTimeMillis());
  Files.setLastModifiedTime(path, now); 
```

## 2。传统 IO—更新文件的上次修改时间。

对于遗留 IO `java.io.File`，我们可以使用`File.setLastModified()`来更新最后修改日期。

UpdateLastModifiedTime2.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.io.IOException;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class UpdateLastModifiedTime2 {

    private static final SimpleDateFormat sdf = new SimpleDateFormat("dd/MM/yyyy");

    public static void main(String[] args) throws ParseException {

        String fileName = "c:\\test\\google.png";

        File file = new File(fileName);

        // print original last modified time
        System.out.println("[original] lastModifiedTime:" + sdf.format(file.lastModified()));

        //need convert the above date to milliseconds in long value
        Date newLastModified = sdf.parse("31/08/1998");

        // update last modified date
        file.setLastModified(newLastModified.getTime());

        //print the latest last modified date
        System.out.println("[updated] lastModifiedTime:" + sdf.format(file.lastModified()));

    }

} 
```

输出

Terminal

```java
 [original] lastModifiedTime:30/09/1998
[updated] lastModifiedTime:31/08/1998 
```

查看`File.setLastModified`方法签名；自纪元(1970 年 1 月 1 日 00:00:00 GMT)以来，参数`long time`以毫秒为单位进行测量。

File.java

```java
 package java.io;

  //...
  public boolean setLastModified(long time) {
      if (time < 0) throw new IllegalArgumentException("Negative time");
      SecurityManager security = System.getSecurityManager();
      if (security != null) {
          security.checkWrite(path);
      }
      if (isInvalid()) {
          return false;
      }
      return fs.setLastModifiedTime(this, time);
  } 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003340/https://github.com/mkyong/core-java)

$ cd java-io

## 参考文献

*   [FileTime JavaDoc](http://web.archive.org/web/20220619003340/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/attribute/FileTime.html)
*   如何将 LocalDate 转换成 Instant？
*   [获取文件在 Java 中的最后修改日期](/web/20220619003340/https://mkyong.com/java/how-to-get-the-file-last-modified-date-in-java/)
*   [管理文件元数据](http://web.archive.org/web/20220619003340/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)
*   [维基百科–纪元时间](http://web.archive.org/web/20220619003340/https://en.wikipedia.org/wiki/Epoch_(computing))
*   [如何在 Java 中格式化 FileTime](/web/20220619003340/https://mkyong.com/java/how-to-format-filetime-in-java/)

<input type="hidden" id="mkyong-current-postId" value="3348">