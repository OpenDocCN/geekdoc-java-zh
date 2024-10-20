# 如何用 Java 读取文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-read-a-file/>

本文重点介绍几种在 Java 中读取文件的常用方法。

1.  `Files.lines`，返回一个`Stream` (Java 8)
2.  `Files.readString`，返回一个`String` (Java 11)，最大文件大小 2G。
3.  `Files.readAllBytes`，返回一个`byte[]` (Java 7)，最大文件大小 2G。
4.  `Files.readAllLines`，返回一个`List<String>` (Java 8)
5.  `BufferedReader`，经典老朋友(Java 1.1 - >永远)
6.  `Scanner` (Java 1.5)

新的 Java 8 `Files.lines`在读取小型或大型文本文件方面表现良好，返回一个`Stream`(灵活的类型并支持并行)，自动关闭资源，并且有一行干净的代码。

```java
 Stream<String> lines = Files.lines(Paths.get(fileName)); 
```

在现代 Java 8+中，我们应该使用`Files.lines`来读取一个文本文件。

**注**
简单来说，小档案阅读没有太大区别，只是回归类型的味道。对于读取大文件，选择 Java 8 `Files.lines`的流和并行特性或经典的`BufferedReader`。

## 文本文件

这里有一个简单的文本文件，只包含五行。其余的 Java 示例将会读取这个文件。

src/main/resources/app.log

```java
 Line 1
Line 2
Line 3
Line 4
Line 5 
```

## 1.Files.lines (Java 8)

1.1 本例使用 Java 8 `Files.lines`将上述文件读入一个`Stream`，并逐行打印。此外，`Files.lines`将帮助自动关闭打开的资源(文件)；我们不需要用 [try-with-resources](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 包装代码。

ReadFile1.java

```java
 package com.mkyong.io.file;

import com.mkyong.io.utils.ResourceHelper;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.stream.Stream;

public class ReadFile1 {

    public static void main(String[] args) throws IOException {

        String fileName = ResourceHelper.getAbsoluteFilePath("app.log");

        // auto-close the resources
        Stream<String> lines = Files.lines(Paths.get(fileName));

        // does not preserve order
        lines.forEach(System.out::println);

        // preserve order
        // lines.forEachOrdered(System.out::println);

    }

} 
```

输出

Terminal

```java
 Line 1
Line 2
Line 3
Line 4
Line 5 
```

1.2 为了读入一个小的文本文件，我们可以使用`collect`很容易地将流转换成`List<String>`。

```java
 String fileName = ResourceHelper.getAbsoluteFilePath("app.log");

  Stream<String> lines = Files.lines(Paths.get(fileName));

  // only for small text file
  List<String> collect = lines.collect(Collectors.toList()); 
```

1.3 对于在大型文本文件中读取，如果行的顺序不重要，我们可以打开流的`parallel`功能来进一步提高读取速度。

```java
 // a large text file, for example, 10G
  String fileName = "/home/mkyong/large.txt";

  Stream<String> lines = Files.lines(Paths.get(fileName));

  // parallel
  lines.parallel().forEach(l -> {/* do work */}); 
```

1.4 一个常见的错误是直接把大的`Stream`转换成`List`，如果流大小大于正在运行的 JVM 堆大小就会抛出`java.lang.OutOfMemoryError: Java heap space`。

```java
 // a large text file, for example, 10G
  String fileName = "/home/mkyong/large.txt";

  Stream<String> lines = Files.lines(Paths.get(fileName));

  // java.lang.OutOfMemoryError: Java heap space
  List<String> collect = lines.collect(Collectors.toList()); 
```

1.5 最后一个`lines.forEach`，不保持行的顺序，如果我们想保持顺序就试试`lines.forEachOrdered`。

```java
 Stream<String> lines = Files.lines(Paths.get(fileName));

  // does not preserve order
  lines.forEach(System.out::println);

  // preserve order
  lines.forEachOrdered(System.out::println); 
```

## 2.Files.readString (Java 11)

2.1 这个`Files.readString()`将一个文件读入一个字符串，如果读取的文件大小超过 2G，就会抛出`java.lang.OutOfMemoryError: Required array size too large`。

ReadFile2.java

```java
 package com.mkyong.io.file;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class ReadFile2 {

    public static void main(String[] args) throws IOException {

        String fileName = "/home/mkyong/large.txt";

        // default UTF_8
        String s = Files.readString(Paths.get(fileName));
        System.out.println(s);

        // String s = Files.readString(Paths.get(fileName), StandardCharsets.UTF_8);
    }
} 
```

2.2 查看源代码，新的 Java 11 `readString`，内部使用已有的 Java 7 `readAllBytes`将一个文件读入一个`byte[]`和`JLA.newStringNoRepl`将`byte[]`转换回一个字符串。

Files.java

```java
 package java.nio.file;

  public final class Files {

    public static String readString(Path path, Charset cs) throws IOException {
          Objects.requireNonNull(path);
          Objects.requireNonNull(cs);

          byte[] ba = readAllBytes(path);

          if (path.getClass().getModule() != Object.class.getModule())
              ba = ba.clone();
          return JLA.newStringNoRepl(ba, cs);
    }

    //...
} 
```

## 3.Files.readAllBytes (Java 7)

3.1 本例使用`Files.readAllBytes`将一个文件读入一个字节数组`byte[]`，如果读取的文件大小超过 2G，就会抛出`java.lang.OutOfMemoryError: Required array size too large`。

ReadFile3.java

```java
 import com.mkyong.io.utils.ResourceHelper;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;

public class ReadFile3 {

    public static void main(String[] args) throws IOException {

        String fileName = "/home/mkyong/app.log";

        byte[] bytes = Files.readAllBytes(Paths.get(fileName));
        String content = new String(bytes, StandardCharsets.UTF_8);
        System.out.println(content);

    }
} 
```

## 4.Files.readAllLines (Java 8)

4.1 这个例子使用`Files.readAllLines`将一个文件读入一个`List<String>`，如果文件大小大于正在运行的 JVM 堆大小，就会抛出`java.lang.OutOfMemoryError: Java heap space`。

ReadFile4.java

```java
 package com.mkyong.io.file;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class ReadFile4 {

    public static void main(String[] args) throws IOException {

        String fileName = "/home/mkyong/app.log";

        List<String> lines = Files.readAllLines(Paths.get(fileName),
                                                  StandardCharsets.UTF_8);
        lines.forEach(System.out::println);

    }
} 
```

## 5.buffer eader(Java 1.1)

5.1 一个经典的老朋友，`BufferedReader`的例子，在读取小文件和大文件时工作良好，默认的缓冲区大小(8k)对于大多数目的来说足够大了。

ReadFile5a.java

```java
 package com.mkyong.io.file;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadFile5a {

    public static void main(String[] args) throws IOException {

        String fileName = "/home/mkyong/app.log";

        // defaultCharBufferSize = 8192; or 8k
        try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        }

    }
} 
```

5.2 我们可以指定缓冲区的大小。

ReadFile5b.java

```java
 package com.mkyong.io.file;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadFile5b {

    public static void main(String[] args) throws IOException {

        String fileName = "/home/mkyong/app.log";

        int bufferSize = 10240; //10k
        try (BufferedReader br = new BufferedReader(
                      new FileReader(fileName), bufferSize)) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        }
    }
} 
```

5.3 在 Java 8 中，我们可以使用新的`Files.newBufferedReader`来创建一个`BufferedReader`。

```java
 try (BufferedReader br = Files.newBufferedReader(Paths.get(fileName))) {
      String line;
      while ((line = br.readLine()) != null) {
          System.out.println(line);
      }
  } 
```

查看源代码，没什么特别的。

Files.java

```java
 package java.nio.file;

public final class Files {

    public static BufferedReader newBufferedReader(Path path, Charset cs)
          throws IOException
      {
          CharsetDecoder decoder = cs.newDecoder();
          Reader reader = new InputStreamReader(newInputStream(path), decoder);
          return new BufferedReader(reader);
      }

    //
} 
```

阅读这个-[Java 如何用 BufferedReader 读取文件](/web/20220619003336/https://mkyong.com/java/how-to-read-file-from-java-bufferedreader-example/)

5.4 对于自引用，一个经典的 try catch finally 手动关闭一个打开的文件。

ReadFile5c.java

```java
 package com.mkyong.io.file;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class ReadFile5c {

    public static void main(String[] args) throws IOException {

        String fileName = "/home/mkyong/app.log";

        BufferedReader br = null;
        FileReader fr = null;

        try {

            fr = new FileReader(fileName);
            br = new BufferedReader(fr);

            // read line by line
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }

        } catch (IOException e) {
            System.err.format("IOException: %s%n", e);
        } finally {
            try {
                if (br != null)
                    br.close();

                if (fr != null)
                    fr.close();
            } catch (IOException ex) {
                System.err.format("IOException: %s%n", ex);
            }
        }

    }
} 
```

## 6.扫描仪(Java 1.5)

6.1 在`Scanner`类中，`delimiter`特性对于读取和过滤小文件仍然很有用。Java 9 和 Java 10 增加了像`findAll`和构造函数这样的新方法来改进`Scanner`类。然而，对于读取大文件，这个`Scanner`类比`BufferedReader`慢。

ReadFile6.java

```java
 package com.mkyong.io.file;

import java.io.FileReader;
import java.io.IOException;
import java.util.Scanner;

public class ReadFile6 {

    public static void main(String[] args) throws IOException {

        String fileName = "/home/mkyong/app.log";

        try (Scanner sc = new Scanner(new FileReader(fileName))) {
            while (sc.hasNextLine()) {
                String line = sc.nextLine();
            }
        }

    }
} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件 JavaDocs](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)
*   [BufferedReader JavaDocs](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)
*   [扫描仪 JavaDocs](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)
*   [Java 教程——阅读、编写和创建文件](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/file.html)
*   [如何在 Java 中读取文件–buffered reader](/web/20220619003336/https://mkyong.com/java/how-to-read-file-from-java-bufferedreader-example/)
*   [Java–从资源文件夹中读取文件](/web/20220619003336/https://mkyong.com/java/java-read-a-file-from-resources-folder/)

<input type="hidden" id="mkyong-current-postId" value="15031">