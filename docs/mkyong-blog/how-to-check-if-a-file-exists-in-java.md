# 如何在 Java 中检查文件是否存在

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-check-if-a-file-exists-in-java/>

在 Java 中，我们可以使用`Files.exists(path)`来测试一个文件是否存在。`path`可以是一个文件或一个目录。最好结合`!Files.isDirectory(path)`来保证现有文件不是一个目录。

```java
 Path path = Paths.get("/home/mkyong/test/test.log");

  // file exists and it is not a directory
  if(Files.exists(path) && !Files.isDirectory(path)) {
      System.out.println("File exists!");
  } 
```

## 1.Files.exists(路径)(NIO)

这个例子使用`Files.exists(path)`来测试一个文件是否存在。

FileExist.java

```java
 package com.mkyong.io.file;

import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class FileExist {

    public static void main(String[] args) {

        Path path = Paths.get("/home/mkyong/test/test.log");

        // check exists for file and directory
        if (Files.exists(path)) {

            if (Files.isRegularFile(path)) {
                System.out.println("File exists!");
            }
            if (Files.isDirectory(path)) {
                System.out.println("File exists, but it is a directory.");
            }

        } else {
            System.out.println("File doesn't exist");
        }

    }
} 
```

1.2 如果文件是一个[符号链接](http://web.archive.org/web/20220629160041/https://en.wikipedia.org/wiki/Symbolic_link)或软链接，默认情况下`Files.exists`将跟随该链接。`Files.exists`有两个参数，我们可以通过第二个`LinkOption.NOFOLLOW_LINKS`参数告诉方法停止跟踪符号链接。

```java
 import java.nio.file.Files;
import java.nio.file.LinkOption;

    // do not follow symbolic link
    if(Files.exists(path, LinkOption.NOFOLLOW_LINKS)){
      //...
    } 
```

1.3 有一个`Files.notExists()`用来测试不存在的文件。

```java
 if(Files.notExists(path)){
      System.out.println("File doesn't exist");
  } 
```

## 2.File.exists(旧 IO)

遗留 IO `java.io.File`有一个类似的`File.exists()`来测试文件或目录是否存在，但是它不支持符号链接。

FileExist2.java

```java
 package com.mkyong.io.file;

import java.io.File;

public class FileExist2 {

    public static void main(String[] args) {

        File file = new File("/home/mkyong/test/");

        if(file.exists() && !file.isDirectory()){
            System.out.println("File exists!");
        }else{
            System.out.println("File doesn't exist");
        }

    }

} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160041/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件 JavaDocs](http://web.archive.org/web/20220629160041/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220629160041/https://docs.oracle.com/javase/8/docs/api/java/io/File.html)
*   [维基百科–符号链接](http://web.archive.org/web/20220629160041/https://en.wikipedia.org/wiki/Symbolic_link)

<input type="hidden" id="mkyong-current-postId" value="2824">