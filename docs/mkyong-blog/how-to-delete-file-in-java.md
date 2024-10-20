# 如何在 Java 中删除文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-delete-file-in-java/>

在 Java 中，我们可以使用 NIO `Files.delete(Path)`和`Files.deleteIfExists(Path)`来删除文件。

## 1.用 Java NIO 删除文件

1.1`Files.delete(Path)`删除一个文件，不返回任何内容，或者失败时抛出一个异常。

DeleteFile1.java

```java
 package com.mkyong.io.file;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class DeleteFile1 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/app1.log";
        try {
            Files.delete(Paths.get(fileName));
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

如果文件不存在，它抛出`NoSuchFileException`。

```java
 java.nio.file.NoSuchFileException: /home/mkyong/app1.log 
```

1.2`Files.deleteIfExists(Path)`也删除一个文件，但它返回一个布尔值，如果文件删除成功则为真；如果文件不存在，不抛出异常。

DeleteFile2.java

```java
 package com.mkyong.io.file;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class DeleteFile2 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/app.log";

        try {
            boolean result = Files.deleteIfExists(Paths.get(fileName));
            if (result) {
                System.out.println("File is deleted!");
            } else {
                System.out.println("Sorry, unable to delete the file.");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

## 2.用 Java IO 删除文件

2.1 对于遗留文件 IO `java.io.*`，我们可以使用`File.delete()`删除一个文件，如果文件删除成功，它将返回 boolean，true，否则返回 false。

DeleteFile3.java

```java
 package com.mkyong.io.file;

import java.io.File;

public class DeleteFile3 {

    public static void main(String[] args) {

        String fileName = "/home/mkyong/app1.log";

        try {
            File file = new File(fileName);
            if (file.delete()) {
                System.out.println(file.getName() + " is deleted!");
            } else {
                System.out.println("Sorry, unable to delete the file.");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

} 
```

2.2`File.deleteOnExit()`有点特殊，它会在 JVM 正常终止时删除文件。但是，不能保证文件会被删除，请小心使用，或者避免使用这种方法。

```java
 File file = new File(fileName);
  file.deleteOnExit(); 
```

**注意**
遗留文件 IO `java.io.*`有[几个弊端](http://web.archive.org/web/20220619003339/https://docs.oracle.com/javase/tutorial/essential/io/legacy.html)，总是挑 Java 文件 NIO，`java.nio.*`。

## 参考

*   [Java NIO 文件 JavaDoc](http://web.archive.org/web/20220619003339/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [Java IO 文件 JavaDoc](http://web.archive.org/web/20220619003339/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)

<input type="hidden" id="mkyong-current-postId" value="5479">