# 如何用 Java 读取 UTF 8 文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-read-utf-8-encoded-data-from-a-file-java/>

在 Java 中， [InputStreamReader](http://web.archive.org/web/20220724142906/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStreamReader.html) 接受一个[字符集](http://web.archive.org/web/20220724142906/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/charset/Charset.html)来将字节流解码成字符流。我们可以向`InputStreamReader`构造函数传递一个`StandardCharsets.UTF_8`来从 UTF-8 文件中读取数据。

```java
 import java.nio.charset.StandardCharsets;

  //...
  try (FileInputStream fis = new FileInputStream(file);
       InputStreamReader isr = new InputStreamReader(fis, StandardCharsets.UTF_8);
       BufferedReader reader = new BufferedReader(isr)
  ) {

      String str;
      while ((str = reader.readLine()) != null) {
          System.out.println(str);
      }

  } catch (IOException e) {
      e.printStackTrace();
  } 
```

在 Java 7+中，许多文件读取 API 开始接受`charset`作为参数，使得读取 UTF-8 变得非常容易。

```java
 // Java 7
  BufferedReader reader = Files.newBufferedReader(path, StandardCharsets.UTF_8);

  // Java 8
  List<String> list = Files.readAllLines(path, StandardCharsets.UTF_8);

  // Java 8
  Stream<String> lines = Files.lines(path, StandardCharsets.UTF_8);

  // Java 11
  String s = Files.readString(path, StandardCharsets.UTF_8); 
```

## 1.UTF 八号档案

一个 UTF 8 编码文件`c:\\temp\\test.txt`，有中文字符。

![utf-8 file](img/650ab0c3aad0f833fa804c804a6df5fb.png)

## 2.阅读 UTF-8 文件

这个例子展示了几种读取 UTF 8 文件的方法。

```java
 package com.mkyong.io.howto;

import java.io.*;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Stream;

public class UnicodeRead {

    public static void main(String[] args) {

        String fileName = "c:\\temp\\test.txt";

        //readUnicodeJava11(fileName);
        readUnicodeBufferedReader(fileName);
        //readUnicodeFiles(fileName);
        //readUnicodeClassic(fileName);

    }

    // Java 7 - Files.newBufferedReader(path, StandardCharsets.UTF_8)
    // Java 8 - Files.newBufferedReader(path) // default UTF-8
    public static void readUnicodeBufferedReader(String fileName) {

        Path path = Paths.get(fileName);

        // Java 8, default UTF-8
        try (BufferedReader reader = Files.newBufferedReader(path)) {

            String str;
            while ((str = reader.readLine()) != null) {
                System.out.println(str);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static void readUnicodeFiles(String fileName) {

        Path path = Paths.get(fileName);
        try {

            // Java 11
            String s = Files.readString(path, StandardCharsets.UTF_8);
            System.out.println(s);

            // Java 8
            List<String> list = Files.readAllLines(path, StandardCharsets.UTF_8);
            list.forEach(System.out::println);

            // Java 8
            Stream<String> lines = Files.lines(path, StandardCharsets.UTF_8);
            lines.forEach(System.out::println);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // Java 11, adds charset to FileReader
    public static void readUnicodeJava11(String fileName) {

        Path path = Paths.get(fileName);

        try (FileReader fr = new FileReader(fileName, StandardCharsets.UTF_8);
             BufferedReader reader = new BufferedReader(fr)) {

            String str;
            while ((str = reader.readLine()) != null) {
                System.out.println(str);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static void readUnicodeClassic(String fileName) {

        File file = new File(fileName);

        try (FileInputStream fis = new FileInputStream(file);
             InputStreamReader isr = new InputStreamReader(fis, StandardCharsets.UTF_8);
             BufferedReader reader = new BufferedReader(isr)
        ) {

            String str;
            while ((str = reader.readLine()) != null) {
                System.out.println(str);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
} 
```

输出

Terminal

```java
 line 1
line 2
line 3
你好，世界 
```

**延伸阅读**
[如何用 Java 写一个 UTF-8 文件](/web/20220724142906/https://mkyong.com/java/how-to-write-utf-8-encoded-data-into-a-file-java/)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220724142906/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [FileWriter JavaDoc](http://web.archive.org/web/20220724142906/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileWriter.html)
*   [BufferedWriter JavaDoc](http://web.archive.org/web/20220724142906/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/BufferedWriter.html)
*   [InputStreamReader JavaDoc](http://web.archive.org/web/20220724142906/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStreamReader.html)
*   [维基百科–UTF-8](http://web.archive.org/web/20220724142906/https://en.wikipedia.org/wiki/UTF-8)
*   [Charset JavaDoc](http://web.archive.org/web/20220724142906/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/charset/Charset.html)

<input type="hidden" id="mkyong-current-postId" value="1319">