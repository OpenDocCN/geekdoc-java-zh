# Java Files.walk 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-files-walk-examples/>

从 Java 8 开始就有了`Files.walk`API；它有助于在给定的起始路径遍历文件树。

主题

1.  [Files.walk()方法签名](#fileswalk-method-signature)
2.  [列出所有文件](#list-all-files)
3.  [列出所有文件夹或目录](#list-all-folders-or-directories)
4.  [通过文件扩展名查找文件](#find-files-by-file-extension)
5.  [通过文件名查找文件](#find-files-by-filename)
6.  [根据文件大小查找文件](#find-files-by-file-size)
7.  [更新所有文件最后修改日期](#update-all-files-last-modified-date)

## 1。Files.walk()方法签名

查看 [Files.walk](http://web.archive.org/web/20220706091051/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#walk(java.nio.file.Path,int,java.nio.file.FileVisitOption...)) 方法签名。

Files.java

```java
 public static Stream<Path> walk(Path start, FileVisitOption... options)
        throws IOException {

        return walk(start, Integer.MAX_VALUE, options);
    }

    public static Stream<Path> walk(Path start,
                                    int maxDepth,
                                    FileVisitOption... options)
        throws IOException
    { 
```

*   `path`，起始文件或文件夹。
*   `maxDepth`，要搜索的最大目录级别数，如果省略，默认为`Integer.MAX_VALUE`，表示要搜索的`2,147,483,647`个目录级别。简而言之，默认情况下，它将搜索目录的所有级别。如果我们只想从顶层或根目录进行搜索，就使用`1`。
*   `FileVisitOption`告诉我们是否要跟随符号链接，默认是 no。我们可以让`FileVisitOption.FOLLOW_LINKS`跟随符号链接。

## 2。列出所有文件

这个例子使用`Files.walk`列出一个目录中的所有文件，包括所有级别的子目录(默认)。

FilesWalkExample1.java

```java
 package com.mkyong.io.api;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FilesWalkExample1 {

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("C:\\test\\");
        List<Path> paths = listFiles(path);
        paths.forEach(x -> System.out.println(x));

    }

    // list all files from this path
    public static List<Path> listFiles(Path path) throws IOException {

        List<Path> result;
        try (Stream<Path> walk = Files.walk(path)) {
            result = walk.filter(Files::isRegularFile)
                    .collect(Collectors.toList());
        }
        return result;

    }

} 
```

输出

Terminal

```java
 C:\test\bk\logo-new.png
C:\test\bk\New Text Document.txt
C:\test\bk\readme.encrypted.txt
C:\test\bk\readme.txt
C:\test\data\ftp\google.png
C:\test\example.json
C:\test\google-decode.png
C:\test\google.png 
```

如果我们只想列出根目录中的文件，请输入`maxDepth = 1`。

FilesWalkExample1.java

```java
 try (Stream<Path> walk = Files.walk(path, 1)) {
        result = walk.filter(Files::isRegularFile)
                .collect(Collectors.toList());
    } 
```

输出

Terminal

```java
 C:\test\example.json
C:\test\google-decode.png
C:\test\google.png 
```

## 3。列出所有文件夹或目录

```java
 // list all directories from this path
    public static List<Path> listDirectories(Path path) throws IOException {

        List<Path> result;
        try (Stream<Path> walk = Files.walk(path)) {
            result = walk.filter(Files::isDirectory)
                    .collect(Collectors.toList());
        }
        return result;

    } 
```

输出

Terminal

```java
 C:\test
C:\test\bk
C:\test\data
C:\test\data\ftp
C:\test\test1
C:\test\test1\test2 
```

## 4。通过文件扩展名
查找文件

这个例子使用`Files.walk`从一个路径中查找所有文本文件`.txt`。

FilesWalkExample4.java

```java
 package com.mkyong.io.api;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FilesWalkExample4 {

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("C:\\test\\");
        List<Path> paths = findByFileExtension(path, ".txt");
        paths.forEach(x -> System.out.println(x));

    }

    public static List<Path> findByFileExtension(Path path, String fileExtension)
            throws IOException {

        if (!Files.isDirectory(path)) {
            throw new IllegalArgumentException("Path must be a directory!");
        }

        List<Path> result;
        try (Stream<Path> walk = Files.walk(path)) {
            result = walk
                    .filter(Files::isRegularFile)   // is a file
                    .filter(p -> p.getFileName().toString().endsWith(fileExtension))
                    .collect(Collectors.toList());
        }
        return result;

    }

} 
```

输出

Terminal

```java
 C:\test\bk\New Text Document.txt
C:\test\bk\readme.encrypted.txt
C:\test\bk\readme.txt 
```

## 5。通过文件名
查找文件

```java
 public static List<Path> findByFileName(Path path, String fileName)
      throws IOException {

      if (!Files.isDirectory(path)) {
          throw new IllegalArgumentException("Path must be a directory!");
      }

      List<Path> result;
      // walk file tree, no more recursive loop
      try (Stream<Path> walk = Files.walk(path)) {
          result = walk
                  .filter(Files::isReadable)      // read permission
                  .filter(Files::isRegularFile)   // is a file
                  .filter(p -> p.getFileName().toString().equalsIgnoreCase(fileName))
                  .collect(Collectors.toList());
      }
      return result;

  } 
```

## 6。按文件大小查找文件

查找文件大小大于或等于 10MB 的所有文件。

FilesWalkExample6.java

```java
 package com.mkyong.io.api;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

// Files.walk example
public class FilesWalkExample6 {

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("C:\\test\\");

        long fileSizeInBytes = 1024 * 1024 * 10; // 10MB

        List<Path> paths = findByFileSize(path, fileSizeInBytes);
        paths.forEach(x -> System.out.println(x));

    }

    // fileSize in bytes
    public static List<Path> findByFileSize(Path path, long fileSize)
        throws IOException {

        if (!Files.isDirectory(path)) {
            throw new IllegalArgumentException("Path must be a directory!");
        }

        List<Path> result;
        // walk file tree, no more recursive loop
        try (Stream<Path> walk = Files.walk(path)) {
            result = walk
                    .filter(Files::isReadable)              // read permission
                    .filter(p -> !Files.isDirectory(p))     // is a file
                    .filter(p -> checkFileSize(p, fileSize))
                    .collect(Collectors.toList());
        }
        return result;

    }

    private static boolean checkFileSize(Path path, long fileSize) {
        boolean result = false;
        try {
            if (Files.size(path) >= fileSize) {
                result = true;
            }
        } catch (IOException e) {
            System.err.println("Unable to get the file size of this file: " + path);
        }
        return result;
    }

} 
```

## 7。更新所有文件最后修改日期

此示例将所有文件的最后修改日期从路径更新为昨天。

FilesWalkExample7.java

```java
 package com.mkyong.io.api;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.attribute.FileTime;
import java.time.Instant;
import java.time.temporal.ChronoUnit;
import java.util.stream.Stream;

public class FilesWalkExample7 {

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("C:\\test\\");

        Instant yesterday = Instant.now().minus(1, ChronoUnit.DAYS);
        setAllFilesModifiedDate(path, yesterday);

    }

    // set all files' last modified time to this instant
    public static void setAllFilesModifiedDate(Path path, Instant instant)
        throws IOException {

        try (Stream<Path> walk = Files.walk(path)) {
            walk
                    .filter(Files::isReadable)      // read permission
                    .filter(Files::isRegularFile)   // file only
                    .forEach(p -> {
                        try {
                            // set last modified time
                            Files.setLastModifiedTime(p, FileTime.from(instant));
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    });
        }
    }

} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220706091051/https://github.com/mkyong/core-java)

$ cd java-io

## 参考文献

*   [Files.walk JavaDoc](http://web.archive.org/web/20220706091051/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#walk(java.nio.file.Path,int,java.nio.file.FileVisitOption...))
*   [Java Files.find 示例](/web/20220706091051/https://mkyong.com/java/java-files-find-examples/)
*   [Java–更新文件的最后修改日期](/web/20220706091051/https://mkyong.com/java/how-to-change-the-file-last-modified-date-in-java/)

<input type="hidden" id="mkyong-current-postId" value="14881">