# 如何在 Java 中复制目录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-copy-directory-in-java/>

在 Java 中，我们可以使用 Java 1.7 `FileVisitor`或 Apache Commons IO `FileUtils.copyDirectory`来复制一个目录，其中包括它的子目录和文件。

本文展示了用 Java 复制目录的几种常用方法。

1.  `FileVisitor` (Java 7+)
2.  `FileUtils.copyDirectory`(Apache common-io)
3.  使用 Java 7 NIO 和 Java 8 Stream 进行自定义复制。
4.  自定义副本，传统 IO。

**注意**NIO`Files.copy`不支持复制目录内容，我们需要创建一个定制的方法来复制目录内容。

## 1.FileVisitor (Java 7)

这个例子展示了如何使用 [FileVisitor](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/FileVisitor.html) 将目录及其内容从`/home/mkyong/test/`复制到`/home/mkyong/test2/`。

Terminal

```java
 $ tree /home/mkyong/test
test
├── test-a1.log
├── test-a2.log
└── test-b
    ├── test-b1.txt
    ├── test-b2.txt
    ├── test-c
    │   ├── test-c1.log
    │   └── test-c2.log
    └── test-d
        ├── test-d1.log
        └── test-d2.log 
```

1.1 这个类扩展了`SimpleFileVisitor`来提供默认值，并且只覆盖必要的方法。

TreeCopyFileVisitor.java

```java
 package com.mkyong.io.utils;

import java.io.IOException;
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributes;

public class TreeCopyFileVisitor extends SimpleFileVisitor<Path> {

    private Path source;
    private final Path target;

    public TreeCopyFileVisitor(String source, String target) {
        this.source = Paths.get(source);
        this.target = Paths.get(target);
    }

    @Override
    public FileVisitResult preVisitDirectory(Path dir,
        BasicFileAttributes attrs) throws IOException {

        Path resolve = target.resolve(source.relativize(dir));
        if (Files.notExists(resolve)) {
            Files.createDirectories(resolve);
            System.out.println("Create directories : " + resolve);
        }
        return FileVisitResult.CONTINUE;

    }

    @Override
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs)
        throws IOException {

        Path resolve = target.resolve(source.relativize(file));
        Files.copy(file, resolve, StandardCopyOption.REPLACE_EXISTING);
        System.out.println(
                String.format("Copy File from \t'%s' to \t'%s'", file, resolve)
        );

        return FileVisitResult.CONTINUE;

    }

    @Override
    public FileVisitResult visitFileFailed(Path file, IOException exc) {
        System.err.format("Unable to copy: %s: %s%n", file, exc);
        return FileVisitResult.CONTINUE;
    }

} 
```

1.2 这个例子使用`Files.walkFileTree`遍历文件树。

CopyDirectory1.java

```java
 package com.mkyong.io.howto;

import com.mkyong.io.utils.TreeCopyFileVisitor;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class CopyDirectory1 {

    public static void main(String[] args) {

        String fromDirectory = "/home/mkyong/test/";
        String toToDirectory = "/home/mkyong/test2/";

        try {

            copyDirectoryFileVisitor(fromDirectory, toToDirectory);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    public static void copyDirectoryFileVisitor(String source, String target)
        throws IOException {

        TreeCopyFileVisitor fileVisitor = new TreeCopyFileVisitor(source, target);
        Files.walkFileTree(Paths.get(source), fileVisitor);

    }

} 
```

输出

Terminal

```java
 Create directories : /home/mkyong/test2
Copy File from 	'/home/mkyong/test/test-a2.log' to 	'/home/mkyong/test2/test-a2.log'
Copy File from 	'/home/mkyong/test/test-a1.log' to 	'/home/mkyong/test2/test-a1.log'
Create directories : /home/mkyong/test2/test-b
Copy File from 	'/home/mkyong/test/test-b/test-b1.txt' to 	'/home/mkyong/test2/test-b/test-b1.txt'
Create directories : /home/mkyong/test2/test-b/test-c
Copy File from 	'/home/mkyong/test/test-b/test-c/test-c2.log' to 	'/home/mkyong/test2/test-b/test-c/test-c2.log'
Copy File from 	'/home/mkyong/test/test-b/test-c/test-c1.log' to 	'/home/mkyong/test2/test-b/test-c/test-c1.log'
Copy File from 	'/home/mkyong/test/test-b/test-b2.txt' to 	'/home/mkyong/test2/test-b/test-b2.txt'
Create directories : /home/mkyong/test2/test-b/test-d
Copy File from 	'/home/mkyong/test/test-b/test-d/test-d2.log' to 	'/home/mkyong/test2/test-b/test-d/test-d2.log'
Copy File from 	'/home/mkyong/test/test-b/test-d/test-d1.log' to 	'/home/mkyong/test2/test-b/test-d/test-d1.log'
Done 
```

检查新目录。

Terminal

```java
 $ tree /home/mkyong/test2
test2
├── test-a1.log
├── test-a2.log
└── test-b
    ├── test-b1.txt
    ├── test-b2.txt
    ├── test-c
    │   ├── test-c1.log
    │   └── test-c2.log
    └── test-d
        ├── test-d1.log
        └── test-d2.log 
```

**注意**
官方 [Java 复制示例](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java)复制一切，包括文件属性。然而，它更难阅读；此示例将方法简化为仅复制文件和目录，并排除属性。

## 2.fileutils . copy directory(Apache commons-io)

这个例子使用了`FileUtils.copyDirectory`来复制一个目录及其内容，这是一个友好而简单的 API。

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

```java
 import org.apache.commons.io.FileUtils;

  //...

  public static void copyFileCommonIO(String from, String to)
      throws IOException {

      File fromDir = new File(from);
      File toDir = new File(to);

      FileUtils.copyDirectory(fromDir, toDir);

  } 
```

## 3.Java NIO 和 Stream。

这个例子使用 Java 8 `Files.list`来模拟文件遍历器，使用 Java 7 NIO `Files`来检查、创建和复制目录及其内容。

CopyDirectory3.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;
import java.util.stream.Stream;

public class CopyDirectory3 {

    public static void main(String[] args) {

        String fromDirectory = "/home/mkyong/test/";
        String toToDirectory = "/home/mkyong/test2/";

        try {

            copyDirectoryJavaNIO(Paths.get(fromDirectory),
                Paths.get(toToDirectory));

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    public static void copyDirectoryJavaNIO(Path source, Path target)
            throws IOException {

        // is this a directory?
        if (Files.isDirectory(source)) {

            //if target directory exist?
            if (Files.notExists(target)) {
                // create it
                Files.createDirectories(target);
                System.out.println("Directory created : " + target);
            }

            // list all files or folders from the source, Java 1.8, returns a stream
            // doc said need try-with-resources, auto-close stream
            try (Stream<Path> paths = Files.list(source)) {

                // recursive loop
                paths.forEach(p ->
                        copyDirectoryJavaNIOWrapper(
                          p, target.resolve(source.relativize(p)))
                );

            }

        } else {
            // if file exists, replace it
            Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);
            System.out.println(
                    String.format("Copy File from \t'%s' to \t'%s'", source, target)
            );
        }
    }

    // extract method to handle exception in lambda
    public static void copyDirectoryJavaNIOWrapper(Path source, Path target) {

        try {
            copyDirectoryJavaNIO(source, target);
        } catch (IOException e) {
            System.err.println("IO errors : " + e.getMessage());
        }

    }

} 
```

## 4.传统 IO

这个例子类似于方法 3。相反，它坚持使用传统的 IO `java.io.*`，仅供参考。

CopyDirectory4.java

```java
 package com.mkyong.io.howto;

import java.io.*;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;
import java.util.stream.Stream;

public class CopyDirectory4 {

    public static void main(String[] args) {

        String fromDirectory = "/home/mkyong/test/";
        String toToDirectory = "/home/mkyong/test2/";

        try {

            copyDirectoryLegacyIO(
                new File(fromDirectory),
                new File(toToDirectory));

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    public static void copyDirectoryLegacyIO(File source, File target)
        throws IOException {

        if (source.isDirectory()) {

            //if directory not exists, create it
            if (!target.exists()) {
                if (target.mkdir()) {
                    System.out.println("Directory copied from "
                            + source + "  to " + target);
                } else {
                    System.err.println("Unable to create directory : " + target);
                }
            }

            // list all the directory contents, file walker
            String[] files = source.list();
            if (files == null) {
                return;
            }

            for (String file : files) {
                //construct the src and dest file structure
                File srcFile = new File(source, file);
                File destFile = new File(target, file);
                //recursive copy
                copyDirectoryLegacyIO(srcFile, destFile);
            }

        } else {

            //if file, then copy it
            //Use bytes stream to support all file types
            InputStream in = null;
            OutputStream out = null;

            try {

                in = new FileInputStream(source);
                out = new FileOutputStream(target);

                byte[] buffer = new byte[1024];

                int length;
                //copy the file content in bytes
                while ((length = in.read(buffer)) > 0) {
                    out.write(buffer, 0, length);
                }

                System.out.println("File copied from " + source + " to " + target);

            } catch (IOException e) {

                System.err.println("IO errors : " + e.getMessage());

            } finally {
                if (in != null) {
                    in.close();
                }

                if (out != null) {
                    out.close();
                }
            }
        }

    }

} 
```

**注**
Java，在 [20+年](http://web.archive.org/web/20220619003336/https://en.wikipedia.org/wiki/Java_version_history)之后，仍然没有官方 API 来复制一个目录，创建一个`Files.copyDirectory()`有多难？

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Java 复制示例](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java)
*   [文件 JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [InputStream JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStream.html)
*   [OutputStream JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/OutputStream.html)
*   [FileVisitor JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/FileVisitor.html)
*   Apache common me
*   [如何在 Java 中复制文件](/web/20220619003336/https://mkyong.com/java/how-to-copy-file-in-java/)
*   Java——如何列出一个目录中的所有文件？

<input type="hidden" id="mkyong-current-postId" value="5585">