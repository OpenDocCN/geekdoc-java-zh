# 如何在 Java 中删除临时文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-delete-temporary-file-in-java/>

在 Java 中，我们可以使用 NIO `Files.delete()`或`Files.deleteIfExists()`来删除一个临时文件，它的工作原理就像[删除一个普通的文本文件](/web/20220629160040/https://mkyong.com/java/how-to-delete-file-in-java/)一样。

```java
 // create a temporary file
  Path path = Files.createTempFile(null, ".log");
  // delete
  Files.delete(path);

  // or
  if(Files.deleteIfExists(path)){
    // success
  }else{
    // file does not exist
  } 
```

## 1.删除临时文件–Java NIO

这个例子使用`java.nio`创建一个临时文件，写一行，然后删除它。

TempFileDelete1.java

```java
 package com.mkyong.io.temp;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class TempFileDelete1 {

    public static void main(String[] args) {

        try {

            Path temp = Files.createTempFile(null, ".log");
            System.out.println(temp);

            // if file doesn't exist throws NoSuchFileException
            // Files.delete(temp);

            // write a line
            Files.write(temp, "Hello World".getBytes(StandardCharsets.UTF_8));

            // check if file exists before delete
            boolean result = Files.deleteIfExists(temp);
            if (result) {
                System.out.println("File is success delete.");
            } else {
                System.out.println("File doesn't exist.");
            }

        } catch (IOException e) {
            System.err.println("Unable to delete the file!");
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 /tmp/16821893865655843803.log
File is success delete. 
```

## 2.删除临时文件 Java IO

这个例子使用遗留 IO `java.io`创建一个临时文件，并在以后删除它。

TempFileDelete2.java

```java
 package com.mkyong.io.temp;

import java.io.File;
import java.io.IOException;

public class TempFileDelete2 {

    public static void main(String[] args) {

        try {

            File tempFile = File.createTempFile("abc_", ".log");
            System.out.println(tempFile);

            boolean result = tempFile.delete();
            if (result) {
                System.out.println(tempFile.getName() + " is deleted!");
            } else {
                System.out.println("Sorry, unable to delete the file.");
            }

            // delete when JVM exit normally.
            // tempFile.deleteOnExit();

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

只有当“JVM 正常退出”小心使用临时文件时，`file.deleteOnExit()`才会删除它。

**注意**
[遗留 IO 有一些缺点](http://web.archive.org/web/20220629160040/https://docs.oracle.com/javase/tutorial/essential/io/legacy.html)，如果可能的话，总是挑选新的 Java NIO `java.nio.*`来做文件 IO 的东西。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160040/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [遗留文件 I/O 代码](http://web.archive.org/web/20220629160040/https://docs.oracle.com/javase/tutorial/essential/io/legacy.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220629160040/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [如何在 Java 中删除文件](/web/20220629160040/https://mkyong.com/java/how-to-delete-file-in-java/)

<input type="hidden" id="mkyong-current-postId" value="5577">