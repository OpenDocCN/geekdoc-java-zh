# Java–如何将 byte[]保存到文件中

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-convert-array-of-bytes-into-file/>

本文展示了几种将`byte[]`保存到文件中的方法。

对于 JDK 1.7 和更高版本，NIO [Files.write](http://web.archive.org/web/20220629161539/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#write(java.nio.file.Path,byte%5B%5D,java.nio.file.OpenOption...)) 是将`byte[]`保存到文件的最简单的解决方案。

```java
 // bytes = byte[]
  Path path = Paths.get("/path/file");
  Files.write(path, bytes); 
```

`FileOutputStream`是最好的选择。

```java
 try (FileOutputStream fos = new FileOutputStream("/path/file")) {
      fos.write(bytes);
      //fos.close // no need, try-with-resources auto close
  } 
```

如果我们有 [Apache Commons IO](http://web.archive.org/web/20220629161539/https://commons.apache.org/proper/commons-io/) ，试试`FileUtils`。

```java
 FileUtils.writeByteArrayToFile(new File("/path/file"), bytes); 
```

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

## 1.将字节[]保存到文件。

下面的例子读取一个文件并将数据转换成一个`byte[]`。稍后，我们将`byte[]`保存到另一个文件中。

ByteToFile.java

```java
 package com.mkyong.io.howto;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class ByteToFile {

    public static void main(String[] args) {

        try {

            // tested with character data and binary data

            // file to bytes[]
            byte[] bytes = Files.readAllBytes(Paths.get("/home/mkyong/test/file.txt"));

            // save byte[] to a file
            writeBytesToFile("/home/mkyong/test/file2.txt", bytes);
            writeBytesToFileNio("/home/mkyong/test/file3.txt", bytes);
            writeBytesToFileApache("/home/mkyong/test/file4.txt", bytes);

            System.out.println("Done");

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    //JDK 7 - FileOutputStream + try-with-resources
    private static void writeBytesToFile(String fileOutput, byte[] bytes)
        throws IOException {

        try (FileOutputStream fos = new FileOutputStream(fileOutput)) {
            fos.write(bytes);
        }

    }

    //JDK 7, NIO, Files.write
    private static void writeBytesToFileNio(String fileOutput, byte[] bytes)
        throws IOException {

        Path path = Paths.get(fileOutput);
        Files.write(path, bytes);

    }

    // Apache Commons IO
    private static void writeBytesToFileApache(String fileOutput, byte[] bytes)
        throws IOException {

        FileUtils.writeByteArrayToFile(new File(fileOutput), bytes);

    }

} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629161539/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Apache Commons FileUtils JavaDoc](http://web.archive.org/web/20220629161539/https://commons.apache.org/proper/commons-io/apidocs/org/apache/commons/io/FileUtils.html#writeByteArrayToFile-java.io.File-byte:A-)
*   Apache common me
*   [FileOutputStream JavaDoc](http://web.archive.org/web/20220629161539/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileOutputStream.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220629161539/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [Java–如何将文件转换成字节数组](/web/20220629161539/https://mkyong.com/java/how-to-convert-file-into-an-array-of-bytes/)

<input type="hidden" id="mkyong-current-postId" value="4153">