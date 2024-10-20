# 如何用 Java 编写 UTF 8 文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-write-utf-8-encoded-data-into-a-file-java/>

在 Java 中， [OutputStreamWriter](http://web.archive.org/web/20220724144027/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/OutputStreamWriter.html) 接受一个[字符集](http://web.archive.org/web/20220724144027/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/charset/Charset.html)来将字符流编码成字节流。我们可以将一个`StandardCharsets.UTF_8`传递给`OutputStreamWriter`构造函数，将数据写入一个 [UTF-8](http://web.archive.org/web/20220724144027/https://en.wikipedia.org/wiki/UTF-8) 文件。

```java
 try (FileOutputStream fos = new FileOutputStream(file);
       OutputStreamWriter osw = new OutputStreamWriter(fos, StandardCharsets.UTF_8);
       BufferedWriter writer = new BufferedWriter(osw)) {

      writer.append(line);

  } 
```

在 Java 7+中，许多文件 I/O 和 NIO 编写器开始接受`charset`作为参数，这使得将数据写入 UTF-8 文件变得非常容易，例如:

```java
 // Java 7
  Files.write(path, lines, StandardCharsets.UTF_8);

  // Java 8
  Files.newBufferedWriter(path) // default UTF-8

  // Java 11
  new FileWriter(new File(fileName), StandardCharsets.UTF_8); 
```

## 1.写入 UTF-8 文件

这个例子展示了一些将中文字符写入 UTF 8 文件的方法。

UnicodeWrite.java

```java
 package com.mkyong.io.howto;

import java.io.*;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Arrays;
import java.util.List;

public class UnicodeWrite {

    public static void main(String[] args) {

        String fileName = "c:\\temp\\test.txt";
        List<String> lines = Arrays.asList("line 1", "line 2", "line 3", "你好，世界");

        writeUnicodeJava7(fileName, lines);
        //writeUnicodeJava8(fileName, lines);
        //writeUnicodeJava11(fileName, lines);
        //writeUnicodeClassic(fileName, lines);

        System.out.println("Done");
    }

    // in the old days
    public static void writeUnicodeClassic(String fileName, List<String> lines) {

        File file = new File(fileName);

        try (FileOutputStream fos = new FileOutputStream(file);
             OutputStreamWriter osw = new OutputStreamWriter(fos, StandardCharsets.UTF_8);
             BufferedWriter writer = new BufferedWriter(osw)
        ) {

            for (String line : lines) {
                writer.append(line);
                writer.newLine();
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static void writeUnicodeJava7(String fileName, List<String> lines) {

        Path path = Paths.get(fileName);
        try {
            Files.write(path, lines, StandardCharsets.UTF_8);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // Java 8 - Files.newBufferedWriter(path) - default UTF-8
    public static void writeUnicodeJava8(String fileName, List<String> lines) {

        Path path = Paths.get(fileName);

        try (BufferedWriter writer = Files.newBufferedWriter(path, StandardCharsets.UTF_8)) {

            for (String line : lines) {
                writer.append(line);
                writer.newLine();
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // Java 11 adds Charset to FileWriter
    public static void writeUnicodeJava11(String fileName, List<String> lines) {

        try (FileWriter fw = new FileWriter(new File(fileName), StandardCharsets.UTF_8);
             BufferedWriter writer = new BufferedWriter(fw)) {

            for (String line : lines) {
                writer.append(line);
                writer.newLine();
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

![utf-8 file](img/64fc95a2124a06dd8cb738b0b36b2c6c.png)

**延伸阅读**
[如何在 Java 中读取一个 UTF-8 文件](/web/20220724144027/https://mkyong.com/java/how-to-read-utf-8-encoded-data-from-a-file-java/)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220724144027/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [FileWriter JavaDoc](http://web.archive.org/web/20220724144027/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileWriter.html)
*   [BufferedWriter JavaDoc](http://web.archive.org/web/20220724144027/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/BufferedWriter.html)
*   [output streamwriter JavaDoc](http://web.archive.org/web/20220724144027/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/OutputStreamWriter.html)
*   [Charset JavaDoc](http://web.archive.org/web/20220724144027/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/charset/Charset.html)
*   [维基百科–UTF-8](http://web.archive.org/web/20220724144027/https://en.wikipedia.org/wiki/UTF-8)

<input type="hidden" id="mkyong-current-postId" value="1310">