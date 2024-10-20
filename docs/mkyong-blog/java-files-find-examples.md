# Java 文件.查找示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-files-find-examples/>

从 Java 8 开始就有了`Files.find` API。它从文件树中快速搜索或查找文件。

主题

1.  [Files.find()方法签名](#filesfind-signature)
2.  [通过文件名查找文件](#find-files-by-filename)
3.  [根据文件大小查找文件](#find-files-by-file-size)
4.  [根据最后修改时间](#find-files-by-last-modified-time)查找文件

*在过去，我们总是使用容易出错的递归循环来遍历文件树。这个 Java 8 `Files.find`可以帮你节省很多时间。*

## 1。Files.find()签名

查看`Files.find()`签名。

Files.java

```java
 public static Stream<Path> find(Path start,
                                int maxDepth,
                                BiPredicate<Path, BasicFileAttributes> matcher,
                                FileVisitOption... options)
        throws IOException 
```

*   `path`，起始文件或文件夹。
*   `maxDepth`定义了要搜索的最大目录级别数。如果我们放置`1`，这意味着只搜索顶层或根文件夹，忽略它的所有子文件夹；如果我们想搜索所有的文件夹级别，请输入`Integer.MAX_VALUE`。
*   `BiPredicate<Path, BasicFileAttributes>`用于条件检查或过滤。
*   `FileVisitOption`告诉我们是否要跟随符号链接，默认是 no。我们可以让`FileVisitOption.FOLLOW_LINKS`跟随符号链接。

## 2。通过文件名
查找文件

这个例子使用`Files.find()`来查找与文件名`google.png`匹配的文件，从文件夹`C:\\test`开始，包括它的所有子文件夹。

FileFindExample1.java

```java
 package com.mkyong.io.api;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FileFindExample1 {

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("C:\\test");
        List<Path> result = findByFileName(path, "google.png");
        result.forEach(x -> System.out.println(x));

    }

    public static List<Path> findByFileName(Path path, String fileName)
            throws IOException {

        List<Path> result;
        try (Stream<Path> pathStream = Files.find(path,
                Integer.MAX_VALUE,
                (p, basicFileAttributes) ->
                        p.getFileName().toString().equalsIgnoreCase(fileName))
        ) {
            result = pathStream.collect(Collectors.toList());
        }
        return result;

    }

} 
```

我们还可以使用`Files`API 来进一步检查路径。

FileFindExample1.java

```java
 try (Stream<Path> pathStream = Files.find(path,
         Integer.MAX_VALUE,
         (p, basicFileAttributes) ->{
             // if directory or no-read permission, ignore
             if(Files.isDirectory(p) || !Files.isReadable(p)){
                 return false;
             }
             return p.getFileName().toString().equalsIgnoreCase(fileName);
         })
 ) 
```

## 3。按文件大小查找文件

这个例子使用`Files.find()`来查找文件大小大于或等于 100MB 的文件。

FileFindExample2.java

```java
 package com.mkyong.io.api;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FileFindExample2 {

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("C:\\Users\\mkyong\\Downloads");

        long fileSize = 1024 * 1024 * 100; // 100M

        List<Path> result = findByFileSize(path, fileSize);
        for (Path p : result) {
            System.out.println(String.format("%-40s [fileSize]: %,d", p, Files.size(p)));
        }

    }

    public static List<Path> findByFileSize(Path path, long fileSize)
            throws IOException {

        List<Path> result;
        try (Stream<Path> pathStream = Files.find(path,
                Integer.MAX_VALUE,
                (p, basicFileAttributes) -> {
                    try {
                        if (Files.isDirectory(p)) {   // ignore directory
                            return false;
                        }
                        return Files.size(p) >= fileSize;
                    } catch (IOException e) {
                        System.err.println("Unable to get the file size of path : " + path);
                    }
                    return false;
                }

        )) {
            result = pathStream.collect(Collectors.toList());
        }
        return result;

    }

} 
```

输出样本。

Terminal

```java
 C:\Users\mkyong\Downloads\hello.mp4 [fileSize]: 4,796,543,886
C:\Users\mkyong\Downloads\java.mp4.bk [fileSize]: 5,502,785,778
C:\Users\mkyong\Downloads\ubuntu-20.04.1-desktop-amd64.iso [fileSize]: 2,785,017,856

#... 
```

## 4。按最后修改时间
查找文件

这个例子使用`File.find()`来查找包含上次修改时间等于或晚于昨天的文件。

FileFindExample3.java

```java
 package com.mkyong.io.api;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.BasicFileAttributes;
import java.nio.file.attribute.FileTime;
import java.time.Instant;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.time.temporal.ChronoUnit;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FileFindExample3 {

    private static DateTimeFormatter DATE_FORMATTER
            = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm:ss");

    public static void main(String[] args) throws IOException {

        // find files, LastModifiedTime from yesterday and above
        List<Path> result = findByLastModifiedTime(
                Paths.get("C:\\test"),
                Instant.now().minus(1, ChronoUnit.DAYS));

        for (Path p : result) {

            // formatting...
            BasicFileAttributes attrs = Files.readAttributes(p, BasicFileAttributes.class);
            FileTime time = attrs.lastModifiedTime();
            LocalDateTime localDateTime = time.toInstant()
                    .atZone(ZoneId.systemDefault())
                    .toLocalDateTime();

            System.out.println(String.format("%-40s [%s]", p, localDateTime.format(DATE_FORMATTER)));
        }

    }

    public static List<Path> findByLastModifiedTime(Path path, Instant instant)
            throws IOException {

        List<Path> result;
        try (Stream<Path> pathStream = Files.find(path,
                Integer.MAX_VALUE,
                (p, basicFileAttributes) -> {

                    if(Files.isDirectory(p) || !Files.isReadable(p)){
                        return false;
                    }

                    FileTime fileTime = basicFileAttributes.lastModifiedTime();
                    // negative if less, positive if greater
                    // 1 means fileTime equals or after the provided instant argument
                    // -1 means fileTime before the provided instant argument
                    int i = fileTime.toInstant().compareTo(instant);
                    return i > 0;
                }

        )) {
            result = pathStream.collect(Collectors.toList());
        }
        return result;

    }

} 
```

输出样本并假设今天是`02/12/2020`

Terminal

```java
 C:\test\a.txt                          [02/12/2020 13:01:20]
C:\test\b.txt                          [01/12/2020 12:11:21]

#... 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003351/https://github.com/mkyong/core-java)

$ cd java-io

## 参考文献

*   [管理元数据](http://web.archive.org/web/20220619003351/https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)
*   [Files.find JavaDoc](http://web.archive.org/web/20220619003351/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#find(java.nio.file.Path,int,java.util.function.BiPredicate,java.nio.file.FileVisitOption...))
*   [Files.walk 示例](/web/20220619003351/https://mkyong.com/java/java-files-walk-examples/)
*   [用 Java 格式化文件时间](/web/20220619003351/https://mkyong.com/java/how-to-format-filetime-in-java/)
*   [Java 字符串格式示例](http://web.archive.org/web/20220619003351/https://mkyong.com/java/java-string-format-examples/)

<input type="hidden" id="mkyong-current-postId" value="16385">