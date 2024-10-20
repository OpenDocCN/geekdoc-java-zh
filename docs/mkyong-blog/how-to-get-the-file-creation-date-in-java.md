# 如何在 Java 中获取文件创建日期

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-the-file-creation-date-in-java/>

在 Java 中(@ 1.7 以后)，我们可以使用 NIO `Files.readAttributes`来获取所有的[文件元数据](http://web.archive.org/web/20220619003341/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)，包括文件创建日期。

```java
 Path file = Paths.get(fileName);

  BasicFileAttributes attr = Files.readAttributes(file, BasicFileAttributes.class);

  System.out.println("creationTime: " + attr.creationTime());
  // creationTime: 2020-07-20T09:29:54.627222Z 
```

## 1.Files.readAttributes (NIO)

这个例子使用`Files.readAttributes`来打印文件创建日期。

GetCreationDate1.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.BasicFileAttributes;

public class GetCreationDate1 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/test.txt";

        try {

            Path file = Paths.get(fileName);

            BasicFileAttributes attr =
                Files.readAttributes(file, BasicFileAttributes.class);

            System.out.println("creationTime: " + attr.creationTime());

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

} 
```

输出

```java
 creationTime: 2020-07-20T09:29:54.627222Z 
```

我们也可以使用相同的代码来获得一个目录的创建日期。

**进一步阅读**
阅读此示例以[将 FileTime 转换为另一种日期时间格式](/web/20220619003341/https://mkyong.com/java/how-to-format-filetime-in-java/)。

## 2.Files.getAttribute (NIO)

`Files.readAttributes`将返回所有文件元数据，如创建时间、最后修改时间、文件大小等。

我们可以使用`Files.getAttribute`来返回一个指定的文件元数据，例如，`creationTime`属性。

GetCreationDate2.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.FileTime;

public class GetCreationDate2 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/test";

        try {

            Path file = Paths.get(fileName);

            FileTime creationTime =
                (FileTime) Files.getAttribute(file, "creationTime");

            System.out.println("creationTime: " + creationTime);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

} 
```

## 3.Java 7 之前

这个示例使用`Runtime.getRuntime().exec`发出一个`dir file /tc`系统命令来列出 Windows 上的文件创建日期，并手动解析内容以提取文件创建日期。

*P.S 在 Java 7 之前，没有官方的 API 来获取文件创建日期。*

3.1 查看 Windows 上的`dir /tc`命令。

Terminal

```java
 C:\> dir c:\logfile.log /tc
 Volume in drive C has no label.
 Volume Serial Number is 0410-1EC3

 Directory of c:\

31/05/2010  08:05                14 logfile.log
               1 File(s)             14 bytes
               0 Dir(s)  35,389,460,480 bytes free

C:\> dir /?

Displays a list of files and subdirectories in a directory.

 //...
 /T          Controls which time field displayed or used for sorting
 timefield   C  Creation
             A  Last Access
             W  Last Written 
```

3.2 Java 示例。

GetFileCreation3.java

```java
 package com.mkyong.io.howto;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class GetFileCreation3 {

    public static void main(String[] args) {

        Process proc;
        BufferedReader br = null;

        try {

            proc = Runtime.getRuntime()
                  .exec("cmd /c dir c:\\logfile.log /tc");

            br = new BufferedReader(
                  new InputStreamReader(proc.getInputStream()));

            String data = "";
            //it's quite stupid but work, ignore first 5 lines
            for (int i = 0; i < 6; i++) {
                data = br.readLine();
            }

            System.out.println("Extracted value : " + data);

            //split by space
            StringTokenizer st = new StringTokenizer(data);
            String date = st.nextToken(); //Get date
            String time = st.nextToken(); //Get time

            System.out.println("Creation Date  : " + date);
            System.out.println("Creation Time  : " + time);

        } catch (IOException e) {

            e.printStackTrace();

        } finally {
            if (br != null) {
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
} 
```

输出

Terminal

```java
 Extracted value : 31/05/2010  08:05  14 logfile.log
Creation Date  : 31/05/2010
Creation Time  : 08:05 
```

上面的代码仍然可以工作，但是要获得一个文件创建时间太复杂了，除非你无法升级 JVM 否则，请使用 Java 7+ NIO `Files.readAttributes`。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003341/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Oracle–文件元数据](http://web.archive.org/web/20220619003341/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)
*   [基础文件属性 JavaDoc](http://web.archive.org/web/20220619003341/https://docs.oracle.com/javase/7/docs/api/java/nio/file/attribute/BasicFileAttributes.html)
*   [如何在 Java 中获取文件的最后修改日期](/web/20220619003341/https://mkyong.com/java/how-to-get-the-file-last-modified-date-in-java/)

<input type="hidden" id="mkyong-current-postId" value="3314">