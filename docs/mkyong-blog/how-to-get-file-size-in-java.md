# 如何在 Java 中获取文件大小

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-file-size-in-java/>

在 Java 中，我们可以使用 [Files.size(path)](http://web.archive.org/web/20220706091052/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#size-java.nio.file.Path-) 来获取文件的大小，以字节为单位。

```java
 // size in bytes
  long bytes = Files.size(path); 
```

## 1.文件大小(NIO)

这个例子使用 NIO `Files.size(path)`来打印图像文件的大小(140 kb)。

GetFileSize.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class GetFileSize {

    public static void main(String[] args) {

        // this image is around 140kb
        String fileName = "/home/mkyong/Pictures/temp.png";
        printFileSizeNIO(fileName);

    }

    public static void printFileSizeNIO(String fileName) {

        Path path = Paths.get(fileName);

        try {

            // size of a file (in bytes)
            long bytes = Files.size(path);
            System.out.println(String.format("%,d bytes", bytes));
            System.out.println(String.format("%,d kilobytes", bytes / 1024));

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 143,041 bytes
139 kilobytes 
```

## 2.File.length(传统 IO)

在过去(Java 7 之前)，我们可以使用遗留 IO `File.length()`来获取文件的字节大小。这个例子将字节格式化到 [yottabytes](http://web.archive.org/web/20220706091052/https://en.wikipedia.org/wiki/Yottabyte) ，只是为了好玩。

GetFileSize2

```java
 package com.mkyong.io.howto;

import java.io.File;

public class GetFileSize2 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/Pictures/temp.png";
        printFileSize(fileName);
    }

    public static void printFileSize(String fileName) {

        File file = new File(fileName);

        if (file.exists()) {

            // size of a file (in bytes)
            long bytes = file.length();

            long kilobytes = (bytes / 1024);
            long megabytes = (kilobytes / 1024);
            long gigabytes = (megabytes / 1024);
            long terabytes = (gigabytes / 1024);
            long petabytes = (terabytes / 1024);
            long exabytes = (petabytes / 1024);
            long zettabytes = (exabytes / 1024);
            long yottabytes = (zettabytes / 1024);

            System.out.println(String.format("%,d bytes", bytes));
            System.out.println(String.format("%,d kilobytes", kilobytes));
            System.out.println(String.format("%,d megabytes", megabytes));
            System.out.println(String.format("%,d gigabytes", gigabytes));
            System.out.println(String.format("%,d terabytes", terabytes));
            System.out.println(String.format("%,d petabytes", petabytes));
            System.out.println(String.format("%,d exabytes", exabytes));
            System.out.println(String.format("%,d zettabytes", zettabytes));
            System.out.println(String.format("%,d yottabytes", yottabytes));

        } else {
            System.out.println("File does not exist!");
        }

    }

} 
```

输出

Terminal

```java
 143,041 bytes
139 kilobytes
0 megabytes
0 gigabytes
0 terabytes
0 petabytes
0 exabytes
0 zettabytes
0 yottabytes 
```

截至发稿时，磁盘存储仍以 TB 为单位，是 Pb 级硬盘还是 SSD 存在？

## 参考

*   [维基百科–字节](http://web.archive.org/web/20220706091052/https://en.wikipedia.org/wiki/Byte)
*   [Java 文件元数据](http://web.archive.org/web/20220706091052/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)
*   [Files.size JavaDoc](http://web.archive.org/web/20220706091052/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#size-java.nio.file.Path-)

<input type="hidden" id="mkyong-current-postId" value="5453">