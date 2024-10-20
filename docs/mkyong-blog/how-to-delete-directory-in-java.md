# 如何在 Java 中删除目录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-delete-directory-in-java/>

如果我们在 Java 中使用 NIO `Files.delete`删除一个非空目录，它会抛出`DirectoryNotEmptyException`；对于遗留 IO `File.delete`删除非空目录，它返回 false。标准的解决方案是递归地循环目录，首先删除所有子目录的内容(子文件或子目录)，然后删除父目录。

这个例子展示了在 Java 中删除目录的一些常用方法。

1.  `Files.walkFileTree` + `FileVisitor` (Java 7)
2.  `Files.walk` (Java 8)
3.  `FileUtils.deleteDirectory`(阿帕奇通用 IO)
4.  目录中的递归删除(普通 Java 代码)

## 目录结构。

我们使用`Files`来创建用于测试的目录和文件。

```java
 public static void createDummyFiles() throws IOException {
      Files.createDirectories(Paths.get("/home/mkyong/test2/test3/test4/test5/"));
      Files.write(Paths.get("/home/mkyong/test2/test2.log"), "hello".getBytes());
      Files.write(Paths.get("/home/mkyong/test2/test3/test3.log"), "hello".getBytes());
  } 
```

查看目录结构。

Terminal

```java
 $ tree test2
test2
├── test2.log
└── test3
    ├── test3.log
    └── test4
        └── test5 
```

稍后我们将删除`test2`目录，包括它的所有内容(子目录或子文件)

## 1.删除目录–files . walk filetree(Java 7)

这个例子使用 NIO `Files.walkFileTree` + `SimpleFileVisitor`删除一个目录。`FileVisitor`将访问指定路径的所有子目录和子文件。

DirectoryDelete1.java

```java
 package com.mkyong.io.directory;

import java.io.IOException;
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributes;

public class DirectoryDelete1 {

    public static void createDummyFiles() throws IOException {
        Files.createDirectories(Paths.get("/home/mkyong/test2/test3/test4/test5/"));
        Files.write(Paths.get("/home/mkyong/test2/test2.log"), "hello".getBytes());
        Files.write(Paths.get("/home/mkyong/test2/test3/test3.log"), "hello".getBytes());
    }

    public static void main(String[] args) {

        String dir = "/home/mkyong/test2/";

        try {

            createDummyFiles();

            deleteDirectoryJava7(dir);

            System.out.println("Done");

        } catch (IOException e) {
            System.err.printf("Failed to delete the directory %n%s%n", e);
        }

    }

    public static void deleteDirectoryJava7(String filePath)
        throws IOException {

        Path path = Paths.get(filePath);

        Files.walkFileTree(path,
            new SimpleFileVisitor<>() {

                // delete directories or folders
                @Override
                public FileVisitResult postVisitDirectory(Path dir,
                                                          IOException exc)
                                                          throws IOException {
                    Files.delete(dir);
                    System.out.printf("Directory is deleted : %s%n", dir);
                    return FileVisitResult.CONTINUE;
                }

                // delete files
                @Override
                public FileVisitResult visitFile(Path file,
                                                 BasicFileAttributes attrs)
                                                 throws IOException {
                    Files.delete(file);
                    System.out.printf("File is deleted : %s%n", file);
                    return FileVisitResult.CONTINUE;
                }
            }
        );

    }

} 
```

输出

Terminal

```java
 File is deleted : /home/mkyong/test2/test2.log
Directory is deleted : /home/mkyong/test2/test3/test4/test5
Directory is deleted : /home/mkyong/test2/test3/test4
File is deleted : /home/mkyong/test2/test3/test3.log
Directory is deleted : /home/mkyong/test2/test3
Directory is deleted : /home/mkyong/test2

Done 
```

## 2.删除目录–files . walk(Java 8)

这个例子使用`Files.walk`为目录返回一个`Stream<Path>`，`Stream<Path>`包含指定路径的所有子目录和子文件。

```java
 public static void deleteDirectoryJava8(String dir) throws IOException {

      Path path = Paths.get(dir);

      // read java doc, Files.walk need close the resources.
      // try-with-resources to ensure that the stream's open directories are closed
      try (Stream<Path> walk = Files.walk(path)) {
          walk
                  .sorted(Comparator.reverseOrder())
                  .forEach(DirectoryDelete::deleteDirectoryJava8Extract);
      }

  }

  // extract method to handle exception in lambda
  public static void deleteDirectoryJava8Extract(Path path) {
      try {
          Files.delete(path);
      } catch (IOException e) {
          System.err.printf("Unable to delete this path : %s%n%s", path, e);
      }
  } 
```

## 3.删除目录–FileUtils(通用 IO)

这个例子使用 Apache Common IO `FileUtils.deleteDirectory`删除一个目录。

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

  FileUtils.deleteDirectory(new File(dir)); 
```

## 4.删除目录–普通 Java 代码

这个例子使用遗留 IO `java.io.*`和一个简单的递归循环算法来删除一个目录。

```java
 public static void deleteDirectoryLegacyIO(File file) {

    File[] list = file.listFiles();
    if (list != null) {
        for (File temp : list) {
            //recursive delete
            System.out.println("Visit " + temp);
            deleteDirectoryLegacyIO(temp);
        }
    }

    if (file.delete()) {
        System.out.printf("Delete : %s%n", file);
    } else {
        System.err.printf("Unable to delete file or directory : %s%n", file);
    }

} 
```

输出

Terminal

```java
 Visit /home/mkyong/test2/test2.log
Delete : /home/mkyong/test2/test2.log
Visit /home/mkyong/test2/test3
Visit /home/mkyong/test2/test3/test4
Visit /home/mkyong/test2/test3/test4/test5
Delete : /home/mkyong/test2/test3/test4/test5
Delete : /home/mkyong/test2/test3/test4
Visit /home/mkyong/test2/test3/test3.log
Delete : /home/mkyong/test2/test3/test3.log
Delete : /home/mkyong/test2/test3
Delete : /home/mkyong/test2

Done 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003339/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [遍历文件树](http://web.archive.org/web/20220619003339/https://docs.oracle.com/javase/tutorial/essential/io/walk.html)
*   [删除文件或目录](http://web.archive.org/web/20220619003339/https://docs.oracle.com/javase/tutorial/essential/io/delete.html)
*   [文件 JavaDoc](http://web.archive.org/web/20220619003339/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [FileUtils JavaDoc](http://web.archive.org/web/20220619003339/https://commons.apache.org/proper/commons-io/javadocs/api-2.7/org/apache/commons/io/FileUtils.html)

<input type="hidden" id="mkyong-current-postId" value="5589">