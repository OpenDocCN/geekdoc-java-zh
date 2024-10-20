# 如何用 Java 创建目录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-create-directory-in-java/>

在 Java 中，我们可以使用 NIO `Files.createDirectory`创建一个目录，或者使用`Files.createDirectories`创建一个包含所有不存在的父目录的目录。

```java
 try {

    Path path = Paths.get("/home/mkyong/a/b/c/");

    //java.nio.file.Files;
    Files.createDirectories(path);

    System.out.println("Directory is created!");

  } catch (IOException e) {

    System.err.println("Failed to create directory!" + e.getMessage());

  } 
```

## 1.创建目录–Java NIO

1.1 我们可以使用`Files.createDirectory`来创建一个目录。

*   如果父目录不存在，抛出`NoSuchFileException`。
*   如果目录存在，抛出`FileAlreadyExistsException`。
*   如果 IO 出错，抛出`IOException`。

```java
 Path path = Paths.get("/home/mkyong/test2/");
  Files.createDirectory(path); 
```

1.2 我们可以使用`Files.createDirectories`创建一个包含所有不存在的父目录的目录。

*   如果父目录不存在，请先创建它。
*   如果目录存在，不会引发异常。
*   如果 IO 出错，抛出`IOException`

```java
 Path path = Paths.get("/home/mkyong/test2/test3/test4/");
  Files.createDirectories(path); 
```

1.3 这个例子使用`Files.createDirectories`创建一个目录`/test4/`，其中包含所有不存在的父目录`/test2/test3/`。

DirectoryCreate1.java

```java
 package com.mkyong.io.directory;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class DirectoryCreate1 {

    public static void main(String[] args) {

        String dir = "/home/mkyong/test2/test3/test4/";

        try {

            Path path = Paths.get(file);

            Files.createDirectories(path);

            System.out.println("Directory is created!");

            //Files.createDirectory(path);

        } catch (IOException e) {
            System.err.println("Failed to create directory!" + e.getMessage());
        }

    }
} 
```

## 2.创建目录-传统 IO

对于遗留 IO `java.io.File`，类似的方法是`file.mkdir()`创建一个目录，`file.mkdirs()`创建一个包含所有不存在的父目录的目录。

`file.mkdir()`和`file.mkdirs()`都返回一个布尔值，如果成功创建目录，则为真，否则为假，不抛出异常。

DirectoryCreate2.java

```java
 package com.mkyong.io.directory;

import java.io.File;

public class DirectoryCreate2 {

    public static void main(String[] args) {

        String dir = "/home/mkyong/test2/test3/test4/";

        File file = new File(dir);

        // true if the directory was created, false otherwise
        if (file.mkdirs()) {
            System.out.println("Directory is created!");
        } else {
            System.out.println("Failed to create directory!");
        }

    }

} 
```

在传统 IO 中，创建目录时缺少抛出异常，这使得开发人员很难调试或理解为什么我们不能创建目录，这也是 Java 发布新的`java.nio.Files`来抛出适当异常的原因之一。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160041/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件 JavaDoc](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)

<input type="hidden" id="mkyong-current-postId" value="5582">