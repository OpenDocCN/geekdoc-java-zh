# Java–如何将文件转换成字节[]

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-convert-file-into-an-array-of-bytes/>

在 Java 中，我们可以使用`Files.readAllBytes(path)`将一个`File`对象转换成一个`byte[]`。

```java
 import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

  String filePath = "/path/to/file";

  // file to byte[], Path
  byte[] bytes = Files.readAllBytes(Paths.get(filePath));

  // file to byte[], File -> Path
  File file = new File(filePath);
  byte[] bytes = Files.readAllBytes(file.toPath()); 
```

NIO `Files`类从 Java 7 开始就可用了。

## 1.文件输入流

在 Java 7 之前，我们可以初始化一个预定义大小的新`byte[]`(与文件长度相同)，使用`FileInputStream`将文件数据读入新的`byte[]`。

```java
 // file to byte[], old and classic way, before Java 7
  private static void readFileToBytes(String filePath) throws IOException {

      File file = new File(filePath);
      byte[] bytes = new byte[(int) file.length()];

      FileInputStream fis = null;
      try {

          fis = new FileInputStream(file);

          //read file into bytes[]
          fis.read(bytes);

      } finally {
          if (fis != null) {
              fis.close();
          }
      }

  } 
```

还是这个`try-with-resources`版本。

```java
 private static void readFileToBytes(String filePath) throws IOException {

    File file = new File(filePath);
    byte[] bytes = new byte[(int) file.length()];

    // funny, if can use Java 7, please uses Files.readAllBytes(path)
    try(FileInputStream fis = new FileInputStream(file)){
        fis.read(bytes);
    }

} 
```

## 2.Apache commons-Apache 公用程式

如果我们有 [Apache Commons IO](http://web.archive.org/web/20220706091011/https://commons.apache.org/proper/commons-io/) ，试试`FileUtils`。

```java
 import org.apache.commons.io.FileUtils;

  //...
  File file = new File("/path/file");
  byte[] bytes = FileUtils.readFileToByteArray(file); 
```

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

## 3.将文件转换为 byte[]，反之亦然。

下面的例子使用 NIO `Files.readAllBytes`将一个图像文件读入一个`byte[]`，并使用`Files.write`将`byte[]`保存到一个新的图像文件中。

FileToBytes.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class FileToBytes {

    public static void main(String[] args) {

        try {

            String filePath = "/home/mkyong/test/phone.png";

            // file to bytes[]
            byte[] bytes = Files.readAllBytes(Paths.get(filePath));

            // bytes[] to file
            Path path = Paths.get("/home/mkyong/test/phone2.png");
            Files.write(path, bytes);

            System.out.println("Done");

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220706091011/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件输入流 JavaDoc](http://web.archive.org/web/20220706091011/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileInputStream.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220706091011/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   Apache common me
*   [Java–如何将字节数组保存到文件中](/web/20220706091011/https://mkyong.com/java/how-to-convert-array-of-bytes-into-file/)

<input type="hidden" id="mkyong-current-postId" value="4149">