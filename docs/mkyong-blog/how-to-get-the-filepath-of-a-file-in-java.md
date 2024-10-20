# 如何在 Java 中获取文件的文件路径

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-the-filepath-of-a-file-in-java/>

在 Java 中，对于 NIO [路径](http://web.archive.org/web/20220619003340/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Path.html)，我们可以使用`path.toAbsolutePath()`来获取文件路径；对于遗留 IO [文件](http://web.archive.org/web/20220619003340/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)，我们可以使用`file.getAbsolutePath()`来获取文件路径。

对于包含`.`或`..`的符号链接或文件路径，我们可以使用`path.toRealPath()`或`file.getCanonicalPath()`来获取真正的文件路径。

下面是`File`和`Path`的映射。

1.  `file.getAbsolutePath()` < = > `path.toAbsolutePath()`
2.  `file.getCanonicalPath()` < = > `path.toRealPath()`
3.  `file.getParent()` < = > `path.getParent()`

## 1.获取文件的文件路径(NIO 路径)。

对于`java.nio.file.Path`，我们可以使用下面的 API 来获取一个文件的文件路径。

1.  `path.toAbsolutePath()`–完整的文件路径。
2.  `path.toRealPath())`–对于符号链接或解析路径名中的`.`或`..`符号，默认为跟随链接。
3.  `path.getParent()`–获取路径的父目录。

1.1 路径= `/home/mkyong/test/file.txt`

```java
 Path path = Paths.get("/home/mkyong/test/file.txt");

path                      : /home/mkyong/test/file.txt
path.toAbsolutePath()     : /home/mkyong/test/file.txt
path.getParent()          : /home/mkyong/test
path.toRealPath()         : /home/mkyong/test/file.txt 
```

1.2 Path = `file.txt`，文件参照[当前工作目录](/web/20220619003340/https://mkyong.com/java/how-to-get-the-current-working-directory-in-java/) + `file.txt`。

```java
 Path path = Paths.get("file.txt");

path                      : file.txt
path.toAbsolutePath()     : /home/mkyong/projects/core-java/java-io/file.txt
path.getParent()          : null
path.toRealPath()         : /home/mkyong/projects/core-java/java-io/file.txt 
```

1.3 Path = `/home/mkyong/test/soft-link`，符号链接。

```java
 $ ln -s
  /home/mkyong/projects/core-java/java-io/src/main/java/com/mkyong/io/howto/GetFilePath.java
  /home/mkyong/test/soft-link 
```

```java
 Path path = Paths.get("/home/mkyong/test/soft-link");

path                      : /home/mkyong/test/soft-link
path.toAbsolutePath()     : /home/mkyong/test/soft-link
path.getParent()          : /home/mkyong/test
path.toRealPath()         : /home/mkyong/projects/core-java/java-io/src/main/java/com/mkyong/io/howto/GetFilePath.java 
```

1.4 路径= `/home/mkyong/test/../hello.txt`

```java
 Path path = Paths.get("/home/mkyong/test/../hello.txt");

path                      : /home/mkyong/test/../hello.txt
path.toAbsolutePath()     : /home/mkyong/test/../hello.txt
path.getParent()          : /home/mkyong/test/..
path.toRealPath()         : /home/mkyong/hello.txt 
```

下面 1.5 是一个完整的 Java 例子，用来获取不同`Path`的文件路径。

GetFilePath1.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.LinkOption;
import java.nio.file.Path;
import java.nio.file.Paths;

public class GetFilePath1 {

    public static void main(String[] args) {

        // full path
        Path path1 = Paths.get("/home/mkyong/test/file.txt");
        System.out.println("\n[Path] : " + path1);
        printPath(path1);

        // file name
        Path path2 = Paths.get("file.txt");
        System.out.println("\n[Path] : " + path2);
        printPath(path2);

        // soft or symbolic link
        Path path3 = Paths.get("/home/mkyong/test/soft-link");
        System.out.println("\n[Path] : " + path3);
        printPath(path3);

        // a path contains `..`
        Path path4 = Paths.get("/home/mkyong/test/../hello.txt");
        System.out.println("\n[Path] : " + path4);
        printPath(path4);

    }

    static void printPath(Path path) {

        System.out.printf("%-25s : %s%n", "path", path);
        System.out.printf("%-25s : %s%n", "path.toAbsolutePath()",
                                                path.toAbsolutePath());
        System.out.printf("%-25s : %s%n", "path.getParent()", path.getParent());
        System.out.printf("%-25s : %s%n", "path.getRoot()", path.getRoot());

        try {

            if (Files.notExists(path)) {
                return;
            }

            // default, follow symbolic link
            System.out.printf("%-25s : %s%n", "path.toRealPath()",
                                                  path.toRealPath());
            // no follow symbolic link
            System.out.printf("%-25s : %s%n", "path.toRealPath(nofollow)",
                path.toRealPath(LinkOption.NOFOLLOW_LINKS));

            // alternative to check isSymbolicLink
            /*if (Files.isSymbolicLink(path)) {
                Path link = Files.readSymbolicLink(path);
                System.out.println(link);
            }*/

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 [Path] : /home/mkyong/test/file.txt
path                      : /home/mkyong/test/file.txt
path.toAbsolutePath()     : /home/mkyong/test/file.txt
path.getParent()          : /home/mkyong/test
path.getRoot()            : /
path.toRealPath()         : /home/mkyong/test/file.txt
path.toRealPath(nofollow) : /home/mkyong/test/file.txt

[Path] : file.txt
path                      : file.txt
path.toAbsolutePath()     : /home/mkyong/projects/core-java/java-io/file.txt
path.getParent()          : null
path.getRoot()            : null
path.toRealPath()         : /home/mkyong/projects/core-java/java-io/file.txt
path.toRealPath(nofollow) : /home/mkyong/projects/core-java/java-io/file.txt

[Path] : /home/mkyong/test/soft-link
path                      : /home/mkyong/test/soft-link
path.toAbsolutePath()     : /home/mkyong/test/soft-link
path.getParent()          : /home/mkyong/test
path.getRoot()            : /
path.toRealPath()         : /home/mkyong/projects/core-java/java-io/src/main/java/com/mkyong/io/howto/GetFilePath.java
path.toRealPath(nofollow) : /home/mkyong/test/soft-link

[Path] : /home/mkyong/test/../hello.txt
path                      : /home/mkyong/test/../hello.txt
path.toAbsolutePath()     : /home/mkyong/test/../hello.txt
path.getParent()          : /home/mkyong/test/..
path.getRoot()            : /
path.toRealPath()         : /home/mkyong/hello.txt
path.toRealPath(nofollow) : /home/mkyong/hello.txt 
```

## 2.获取文件的文件路径(旧文件)

对于遗留 IO `java.io.File`，我们可以使用下面的 API 来获取文件的文件路径。

1.  `file.getAbsolutePath()` = `path.toAbsolutePath()`
2.  `file.getCanonicalPath()` = `path.toRealPath()`
3.  `file.getParent()` = `path.getParent()`

GetFilePath2.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.io.IOException;

public class GetFilePath2 {

    public static void main(String[] args) {

        // full file path
        File file1 = new File("/home/mkyong/test/file.txt");
        System.out.println("[File] : " + file1);
        printFilePath(file1);

        // a file name
        File file2 = new File("file.txt");
        System.out.println("\n[File] : " + file2);
        printFilePath(file2);

        // a soft or symbolic link
        File file3 = new File("/home/mkyong/test/soft-link");
        System.out.println("\n[File] : " + file3);
        printFilePath(file3);

        // a file contain `..`
        File file4 = new File("/home/mkyong/test/../hello.txt");
        System.out.println("\n[File] : " + file4);
        printFilePath(file4);

    }

    // If a single file name, not full path, the file refer to
    // System.getProperty("user.dir") + file
    static void printFilePath(File file) {
        // print File = print file.getPath()
        System.out.printf("%-25s : %s%n", "file.getPath()", file.getPath());
        System.out.printf("%-25s : %s%n", "file.getAbsolutePath()",
                                              file.getAbsolutePath());
        try {
            System.out.printf("%-25s : %s%n", "file.getCanonicalPath()",
                                                file.getCanonicalPath());
        } catch (IOException e) {
            e.printStackTrace();
        }
        System.out.printf("%-25s : %s%n", "Parent Path", getParentPath(file));
    }

    // if unable to get parent, try substring to get the parent folder.
    private static String getParentPath(File file) {
        if (file.getParent() == null) {
            String absolutePath = file.getAbsolutePath();
            return absolutePath.substring(0,
                          absolutePath.lastIndexOf(File.separator));
        } else {
            return file.getParent();
        }
    }

} 
```

输出

Terminal

```java
 [File] : /home/mkyong/test/file.txt
file.getPath()            : /home/mkyong/test/file.txt
file.getAbsolutePath()    : /home/mkyong/test/file.txt
file.getCanonicalPath()   : /home/mkyong/test/file.txt
Parent Path               : /home/mkyong/test

[File] : file.txt
file.getPath()            : file.txt
file.getAbsolutePath()    : /home/mkyong/projects/core-java/java-io/file.txt
file.getCanonicalPath()   : /home/mkyong/projects/core-java/java-io/file.txt
Parent Path               : /home/mkyong/projects/core-java/java-io

[File] : /home/mkyong/test/soft-link
file.getPath()            : /home/mkyong/test/soft-link
file.getAbsolutePath()    : /home/mkyong/test/soft-link
file.getCanonicalPath()   : /home/mkyong/projects/core-java/java-io/src/main/java/com/mkyong/io/howto/GetFilePath.java
Parent Path               : /home/mkyong/test

[File] : /home/mkyong/test/../hello.txt
file.getPath()            : /home/mkyong/test/../hello.txt
file.getAbsolutePath()    : /home/mkyong/test/../hello.txt
file.getCanonicalPath()   : /home/mkyong/hello.txt
Parent Path               : /home/mkyong/test/.. 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003340/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件 JavaDoc](http://web.archive.org/web/20220619003340/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)
*   [路径 JavaDoc](http://web.archive.org/web/20220619003340/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Path.html)
*   [Java 当前工作目录](/web/20220619003340/https://mkyong.com/java/how-to-get-the-current-working-directory-in-java/)

<input type="hidden" id="mkyong-current-postId" value="5473">