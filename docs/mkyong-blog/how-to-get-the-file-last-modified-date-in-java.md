# 如何在 Java 中获取文件的最后修改日期

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-the-file-last-modified-date-in-java/>

在 Java 中，我们可以使用`Files.readAttributes()`来获取文件元数据或属性，然后使用`lastModifiedTime()`来显示文件的最后修改日期。

```java
 Path file = Paths.get("/home/mkyong/file.txt");

  BasicFileAttributes attr = Files.readAttributes(file, BasicFileAttributes.class);

  System.out.println("lastModifiedTime: " + attr.lastModifiedTime()); 
```

## 1.基本文件属性(NIO)

这个例子使用`java.nio.*`来显示[文件属性](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)或元数据——创建时间、最后访问时间和最后修改时间。

GetLastModifiedTime1.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.BasicFileAttributes;

public class GetLastModifiedTime1 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/file.txt";

        try {

            Path file = Paths.get(fileName);
            BasicFileAttributes attr =
                Files.readAttributes(file, BasicFileAttributes.class);

            System.out.println("creationTime: " + attr.creationTime());
            System.out.println("lastAccessTime: " + attr.lastAccessTime());
            System.out.println("lastModifiedTime: " + attr.lastModifiedTime());

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 creationTime: 2020-07-20T09:29:54.627222Z
lastAccessTime: 2020-07-21T12:15:56.699971Z
lastModifiedTime: 2020-07-20T09:29:54.627222Z 
```

`BasicFileAttributes`也适用于目录，我们可以使用相同的代码来显示目录的最后修改时间。

**进一步阅读**
阅读此示例以[将 FileTime 转换为另一种日期时间格式](/web/20220619003336/https://mkyong.com/java/how-to-format-filetime-in-java/)。

## 2.File.lastModified(旧 IO)

对于遗留 IO，我们可以使用`File.lastModified()`来获取最后修改的时间；该方法返回自[纪元时间]以来以毫秒为单位测量的长值(https://en . Wikipedia . org/wiki/Epoch _(计算))。我们可以使用`SimpleDateFormat`使它成为一种更易于阅读的格式。

GetLastModifiedTime2.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.text.SimpleDateFormat;

public class GetLastModifiedTime2 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/test";

        File file = new File(fileName);

        System.out.println("Before Format : " + file.lastModified());

        SimpleDateFormat sdf = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");

        System.out.println("After Format : " + sdf.format(file.lastModified()));

    }

} 
```

输出

Terminal

```java
 Before Format : 1595237394627
After Format : 07/20/2020 17:29:54 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [管理文件元数据](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)
*   [维基百科–纪元时间](http://web.archive.org/web/20220619003336/https://en.wikipedia.org/wiki/Epoch_(computing))
*   [如何在 Java 中格式化 FileTime](/web/20220619003336/https://mkyong.com/java/how-to-format-filetime-in-java/)

<input type="hidden" id="mkyong-current-postId" value="2844">