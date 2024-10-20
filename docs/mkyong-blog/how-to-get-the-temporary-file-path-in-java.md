# 如何在 Java 中获取临时文件路径

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-the-temporary-file-path-in-java/>

在 Java 中，我们可以使用`System.getProperty("java.io.tmpdir")`来获取默认的临时文件位置。

1.  对于 Windows，默认的临时文件夹是`%USER%\AppData\Local\Temp`
2.  对于 Linux，默认的临时文件夹是`/tmp`

## 1.java.io.tmpdir

在 Ubuntu Linux 上运行下面的 Java 程序。

TempFilePath1

```java
 package com.mkyong.io.temp;

public class TempFilePath1 {

    public static void main(String[] args) {

        String tmpdir = System.getProperty("java.io.tmpdir");
        System.out.println("Temp file path: " + tmpdir);

    }

} 
```

输出

```java
 Temp file path: /tmp 
```

## 2.创建临时文件

或者，我们可以创建一个临时文件并通过`substring`文件路径来获得临时文件的位置。

2.1 Java NIO 示例。

TempFilePath2.java

```java
 package com.mkyong.io.temp;

import java.io.IOException;
import java.nio.file.FileSystems;
import java.nio.file.Files;
import java.nio.file.Path;

public class TempFilePath2 {

    public static void main(String[] args) {

        // Java NIO
        try {
            Path temp = Files.createTempFile("", ".tmp");

            String absolutePath = temp.toString();
            System.out.println("Temp file : " + absolutePath);

            String separator = FileSystems.getDefault().getSeparator();
            String tempFilePath = absolutePath
                  .substring(0, absolutePath.lastIndexOf(separator));

            System.out.println("Temp file path : " + tempFilePath);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Temp file : /tmp/log_11536339146653799756.tmp
Temp file path : /tmp 
```

2.2 Java IO 示例。

TempFilePath3.java

```java
 package com.mkyong.io.temp;

import java.io.File;
import java.io.IOException;

public class TempFilePath3 {

    public static void main(String[] args) {

        // Java IO
        try {
            File temp = File.createTempFile("log_", ".tmp");
            System.out.println("Temp file : " + temp.getAbsolutePath());

            String absolutePath = temp.getAbsolutePath();
            String tempFilePath = absolutePath
                  .substring(0, absolutePath.lastIndexOf(File.separator));

            System.out.println("Temp file path : " + tempFilePath);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Temp file : /tmp/log_9219838414378386507.tmp
Temp file path : /tmp 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003342/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Java NIO 路径 JavaDoc](http://web.archive.org/web/20220619003342/https://docs.oracle.com/javase/7/docs/api/java/nio/file/Paths.html)
*   [如何在 Java 中创建临时文件](/web/20220619003342/https://mkyong.com/java/how-to-create-temporary-file-in-java/)
*   [如何删除 Windows 中的临时文件](http://web.archive.org/web/20220619003342/https://www.lifewire.com/how-to-delete-temporary-files-in-windows-2624709)
*   [Linux 临时文件](http://web.archive.org/web/20220619003342/https://www.linux.org/threads/temporary-files.10817/)

<input type="hidden" id="mkyong-current-postId" value="5499">