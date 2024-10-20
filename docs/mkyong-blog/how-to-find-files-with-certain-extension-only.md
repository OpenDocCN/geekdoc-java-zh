# 如何在 Java 中查找文件扩展名为的文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-find-files-with-certain-extension-only/>

本文展示了如何用 Java 8 `Files.walk`遍历文件树和流操作`filter`来从文件夹及其子文件夹中查找与特定文件扩展名匹配的文件。

```java
 // find files matched `png` file extension from folder C:\\test
  try (Stream<Path> walk = Files.walk(Paths.get("C:\\test"))) {
      result = walk
              .filter(p -> !Files.isDirectory(p))   // not a directory
              .map(p -> p.toString().toLowerCase()) // convert path to string
              .filter(f -> f.endsWith("png"))       // check end with
              .collect(Collectors.toList());        // collect all matched to a List
  } 
```

在`Files.walk`方法中，第二个参数`maxDepth`定义了要访问的最大目录级别数。我们可以指定`maxDepth = 1`只从顶层文件夹中查找文件(排除它所有的子文件夹)

```java
 try (Stream<Path> walk = Files.walk(Paths.get("C:\\test"), 1)) {
      //...
  } 
```

主题

1.  [查找指定文件扩展名的文件(Files.walk)](#find-files-with-a-specified-file-extension)
2.  [查找具有多个文件扩展名的文件(Files.walk)](#find-files-with-multiple-file-extensions)

## 1。查找具有指定文件扩展名
的文件

本示例查找与文件扩展名`png`匹配的文件。查找从顶层文件夹`C:\\test`开始，包括所有级别的子文件夹。

FindFileByExtension1.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FindFileByExtension1 {

    public static void main(String[] args) {

        try {

            List<String> files = findFiles(Paths.get("C:\\test"), "png");
            files.forEach(x -> System.out.println(x));

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static List<String> findFiles(Path path, String fileExtension)
        throws IOException {

        if (!Files.isDirectory(path)) {
            throw new IllegalArgumentException("Path must be a directory!");
        }

        List<String> result;

        try (Stream<Path> walk = Files.walk(path)) {
            result = walk
                    .filter(p -> !Files.isDirectory(p))
                    // this is a path, not string,
                    // this only test if path end with a certain path
                    //.filter(p -> p.endsWith(fileExtension))
                    // convert path to string first
                    .map(p -> p.toString().toLowerCase())
                    .filter(f -> f.endsWith(fileExtension))
                    .collect(Collectors.toList());
        }

        return result;
    }

} 
```

输出

Terminal

```java
 c:\test\bk\logo-new.png
c:\test\bk\resize-default.png
c:\test\google.png
c:\test\test1\test2\java.png
... 
```

## 2。查找具有多个文件扩展名的文件

本示例查找与多个文件扩展名(`.png`、`.jpg`、`.gif`)匹配的文件。

FindFileByExtension2.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FindFileByExtension2 {

    public static void main(String[] args) {

        try {

            String[] extensions = {"png", "jpg", "gif"};
            List<String> files = findFiles(Paths.get("C:\\test"), extensions);
            files.forEach(x -> System.out.println(x));

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static List<String> findFiles(Path path, String[] fileExtensions) throws IOException {

        if (!Files.isDirectory(path)) {
            throw new IllegalArgumentException("Path must be a directory!");
        }

        List<String> result;
        try (Stream<Path> walk = Files.walk(path, 1)) {
            result = walk
                    .filter(p -> !Files.isDirectory(p))
                    // convert path to string
                    .map(p -> p.toString().toLowerCase())
                    .filter(f -> isEndWith(f, fileExtensions))
                    .collect(Collectors.toList());
        }
        return result;

    }

    private static boolean isEndWith(String file, String[] fileExtensions) {
        boolean result = false;
        for (String fileExtension : fileExtensions) {
            if (file.endsWith(fileExtension)) {
                result = true;
                break;
            }
        }
        return result;
    }

} 
```

输出

Terminal

```java
 c:\test\bk\logo-new.png
c:\test\bk\resize-default.gif
c:\test\bk\resize-fast.gif
c:\test\bk\resize.png
c:\test\google.jpg
c:\test\google.png
c:\test\test1\test2\java.png
c:\test\test1\test2\java.jpg 
```

2.2 使用 Java 8 stream `anyMatch`可以缩短`isEndWith()`方法。

```java
 private static boolean isEndWith(String file, String[] fileExtensions) {

    // Java 8, try this
    boolean result = Arrays.stream(fileExtensions).anyMatch(file::endsWith);
    return result;

    // old school style
    /*boolean result = false;
    for (String fileExtension : fileExtensions) {
        if (file.endsWith(fileExtension)) {
            result = true;
            break;
        }
    }
    return result;*/
} 
```

2.3 我们也可以去掉`isEndWith()`方法，直接将`anyMatch`放入`filter`中。

```java
 public static List<String> findFiles(Path path, String[] fileExtensions)
      throws IOException {

      if (!Files.isDirectory(path)) {
          throw new IllegalArgumentException("Path must be a directory!");
      }

      List<String> result;
      try (Stream<Path> walk = Files.walk(path, 1)) {
          result = walk
                  .filter(p -> !Files.isDirectory(p))
                  // convert path to string
                  .map(p -> p.toString().toLowerCase())
                  //.filter(f -> isEndWith(f, fileExtensions))

                  // lambda
                  //.filter(f -> Arrays.stream(fileExtensions).anyMatch(ext -> f.endsWith(ext)))

                  // method reference
                  .filter(f -> Arrays.stream(fileExtensions).anyMatch(f::endsWith))     
                  .collect(Collectors.toList());
      }
      return result;

  } 
```

2.4 我们可以通过将不同的条件传递到流`filter`中来进一步增强程序；现在，该程序可以很容易地从文件夹中搜索或找到具有指定模式的文件。例如:

查找文件名以“abc”开头的文件。

```java
 List<String> result;
  try (Stream<Path> walk = Files.walk(path)) {
      result = walk
              .filter(p -> !Files.isDirectory(p))
              // convert path to string
              .map(p -> p.toString())
              .filter(f -> f.startsWith("abc"))
              .collect(Collectors.toList());
  } 
```

查找文件名包含单词“mkyong”的文件。

```java
 List<String> result;
  try (Stream<Path> walk = Files.walk(path)) {
      result = walk
              .filter(p -> !Files.isDirectory(p))
              // convert path to string
              .map(p -> p.toString())
              .filter(f -> f.contains("mkyong"))
              .collect(Collectors.toList());
  } 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考文献

*   [FilenameFilter JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FilenameFilter.html)
*   [Files.walk JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#walk(java.nio.file.Path,int,java.nio.file.FileVisitOption...))
*   [Files.walk 示例](/web/20220619003336/https://mkyong.com/java/java-files-walk-examples/)
*   [如何在 Java 中获取文件扩展名](/web/20220619003336/https://mkyong.com/java/how-to-get-file-extension-in-java/)
*   [如何在 Java 中复制目录](/web/20220619003336/https://mkyong.com/java/how-to-copy-directory-in-java/)

<input type="hidden" id="mkyong-current-postId" value="2833">