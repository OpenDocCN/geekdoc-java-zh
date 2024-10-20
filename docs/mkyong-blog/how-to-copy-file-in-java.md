# 如何在 Java 中复制文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-copy-file-in-java/>

本文展示了用 Java 复制文件的四种方法。

1.  `Files.copy`
2.  Apache commons me(Apache 公用程式)
3.  番石榴
4.  普通爪哇咖啡

在 Java 7+中，NIO [Files.copy](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#copy(java.nio.file.Path,java.io.OutputStream)) 是复制文件最简单的方法，因为它是一个内置的 API。在 Java 7 之前，Apache Commons IO `FileUtils.copyFile`是一个更好的解决方案，因为遗留 IO `java.io.*`没有 API 可以复制。

## 1.文件.副本(NIO)

1.1 这个代码片段使用 NIO 来复制文件。默认情况下，如果目标文件存在，复制会失败并抛出`FileAlreadyExistsException`。

```java
 Path fromFile = Paths.get(from);
  Path toFile = Paths.get(to);

  Files.copy(fromFile, toFile); 
```

1.2`Files.copy`接受一个 varargs 参数:

*   `StandardCopyOption.REPLACE_EXISTING`–如果目标文件存在，替换它。
*   `StandardCopyOption.COPY_ATTRIBUTES`–将文件属性复制到目标文件。

```java
 import java.nio.file.StandardCopyOption;

  // if target or destination file exists, replace it.
  Files.copy(fromFile, toFile, StandardCopyOption.REPLACE_EXISTING);

  // multiple StandardCopyOption
  CopyOption[] options = { StandardCopyOption.REPLACE_EXISTING,
                StandardCopyOption.COPY_ATTRIBUTES,
                LinkOption.NOFOLLOW_LINKS };

  Files.copy(fromFile, toFile, options); 
```

下面 1.3 是一个完整的 Java NIO 复制文件的例子。

CopyFile1.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class CopyFile1 {

    public static void main(String[] args) {

        String fromFile = "/home/mkyong/dev/db.debug.conf";
        String toFile = "/home/mkyong/live/db.conf";

        try {
            copyFileNIO(fromFile, toFile);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Copy file is done.");
    }

    public static void copyFileNIO(String from, String to) throws IOException {

        Path fromFile = Paths.get(from);
        Path toFile = Paths.get(to);

        // if fromFile doesn't exist, Files.copy throws NoSuchFileException
        if (Files.notExists(fromFile)) {
            System.out.println("File doesn't exist? " + fromFile);
            return;
        }

        // if toFile folder doesn't exist, Files.copy throws NoSuchFileException
        // if toFile parent folder doesn't exist, create it.
        Path parent = toFile.getParent();
        if(parent!=null){
            if(Files.notExists(parent)){
                Files.createDirectories(parent);
            }
        }

        // default - if toFile exist, throws FileAlreadyExistsException
        Files.copy(fromFile, toFile);

        // if toFile exist, replace it.
        // Files.copy(fromFile, toFile, StandardCopyOption.REPLACE_EXISTING);

        // multiple StandardCopyOption
        /*CopyOption[] options = { StandardCopyOption.REPLACE_EXISTING,
                StandardCopyOption.COPY_ATTRIBUTES,
                LinkOption.NOFOLLOW_LINKS };

        Files.copy(fromFile, toFile, options);*/

    }
} 
```

## 2.Apache commons me(Apache 公用程式)

在过去(Java 7 之前)， [commons-io](http://web.archive.org/web/20220619003336/https://commons.apache.org/proper/commons-io/) 是用 Java 复制文件的最简单的解决方案。

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

```java
 import org.apache.commons.io.FileUtils;

  public static void copyFileCommonIO(String from, String to)
        throws IOException {

        File fromFile = new File(from);
        File toFile = new File(to);

        FileUtils.copyFile(fromFile, toFile);

  } 
```

## 3.谷歌番石榴

很多项目开始采用[的番石榴](http://web.archive.org/web/20220619003336/https://github.com/google/guava)，而更喜欢番石榴的`Files.copy`，与 Java NIO 同名，确保导入正确的包`com.google.common.io.Files`。

pom.xml

```java
 <dependency>
      <groupId>com.google.guava</groupId>
      <artifactId>guava</artifactId>
      <version>29.0-jre</version>
  </dependency> 
```

```java
 public static void copyFileGuava(String from, String to) throws IOException {

      File fromFile = new File(from);
      File toFile = new File(to);

      // @Beta?
      com.google.common.io.Files.copy(fromFile, toFile);

  } 
```

番石榴的`Files`类从 1.0 版本开始就有了，不知道为什么`Files.copy`仍然有一个`@Beta`注释在上面？

Files.java

```java
 package com.google.common.io;

public final class Files {

  //..
  @Beta
  public static void copy(File from, File to) throws IOException {
    checkArgument(!from.equals(to),
      "Source %s and destination %s must be different", from, to);
    asByteSource(from).copyTo(asByteSink(to));
  } 
```

## 4.普通爪哇咖啡

这个例子使用普通的 Java `InputStream`和`OutputStream`来复制一个文件。

```java
 public static void copyFilePlainJava(String from, String to) throws IOException {

      InputStream inStream = null;
      OutputStream outStream = null;

      try {

          File fromFile = new File(from);
          File toFile = new File(to);

          inStream = new FileInputStream(fromFile);
          outStream = new FileOutputStream(toFile);

          byte[] buffer = new byte[1024];

          int length;
          while ((length = inStream.read(buffer)) > 0) {
              outStream.write(buffer, 0, length);
              outStream.flush();
          }

      } finally {
          if (inStream != null)
              inStream.close();

          if (outStream != null)
              outStream.close();
      }

  } 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Java 教程复制文件](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/copy.html)
*   [番石榴–谷歌 Java 核心库](http://web.archive.org/web/20220619003336/https://github.com/google/guava)
*   [InputStream JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStream.html)
*   [OutputStream JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/OutputStream.html)
*   [Commons IO](http://web.archive.org/web/20220619003336/https://commons.apache.org/proper/commons-io/)

<input type="hidden" id="mkyong-current-postId" value="5467">