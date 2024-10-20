# Java–将文件转换为路径

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-convert-file-to-path/>

这篇文章展示了如何在 Java 中将一个[文件](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)转换成一个[路径](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Path.html)。

`java.io.File`=>= 

```java
 // Convert File to Path
File file = new File("/home/mkyong/test/file.txt");
Path path = file.toPath(); 
```

`java.nio.file.Path`=>= 

```java
 // Convert Path to File
Path path = Paths.get("/home/mkyong/test/file.txt");
File file = path.toFile(); 
```

## 1.将文件转换为路径

在 Java 中，我们可以使用`file.toPath()`将一个`File`转换成一个`Path`。

FileToPath.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.nio.file.Path;

public class FileToPath {

    public static void main(String[] args) {

        File file = new File("/home/mkyong/test/file.txt");

        // Java 1.7, convert File to Path
        Path path = file.toPath();

        System.out.println(path);

    }
} 
```

## 2.将路径转换为文件

在 Java 中，我们可以使用`path.toFile()`将一个`Path`转换成一个`File`。

PathToFile.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.nio.file.Path;
import java.nio.file.Paths;

public class PathToFile {

    public static void main(String[] args) {

        Path path = Paths.get("/home/mkyong/test/file.txt");

        // Convert Path to File
        File file = path.toFile();

        System.out.println(file);

    }
} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件 JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)
*   [路径 JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Path.html)

<input type="hidden" id="mkyong-current-postId" value="16146">