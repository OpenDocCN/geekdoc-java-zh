# 如何在 Java 中获取当前工作目录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-get-the-current-working-directory-in-java/>

在 Java 中，我们可以使用`System.getProperty("user.dir")`来获取当前的工作目录，即程序启动的目录。

```java
 String dir = System.getProperty("user.dir");

  // directory from where the program was launched
  // e.g /home/mkyong/projects/core-java/java-io
  System.out.println(dir); 
```

这个系统属性`user.dir`的一个好处是，我们可以通过一个`-D`参数轻松覆盖系统属性，例如:

```java
 java -Duser.dir=/home/mkyong/ -jar abc.jar 
```

## 1.工作目录

下面的程序显示了获取当前工作目录的不同方式，如`File`、`Paths`、`FileSystems`或系统属性；所有方法都将返回相同的结果。

CurrentWorkingDirectory.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.nio.file.FileSystems;
import java.nio.file.Paths;

public class CurrentWorkingDirectory {

    public static void main(String[] args) {

        printCurrentWorkingDirectory1();
        printCurrentWorkingDirectory2();
        printCurrentWorkingDirectory3();
        printCurrentWorkingDirectory4();

    }

    // System Property
    private static void printCurrentWorkingDirectory1() {
        String userDirectory = System.getProperty("user.dir");
        System.out.println(userDirectory);
    }

    // Path, Java 7
    private static void printCurrentWorkingDirectory2() {
        String userDirectory = Paths.get("")
                .toAbsolutePath()
                .toString();
        System.out.println(userDirectory);
    }

    // File("")
    private static void printCurrentWorkingDirectory3() {
        String userDirectory = new File("").getAbsolutePath();
        System.out.println(userDirectory);
    }

    // FileSystems
    private static void printCurrentWorkingDirectory4() {
        String userDirectory = FileSystems.getDefault()
                .getPath("")
                .toAbsolutePath()
                .toString();
        System.out.println(userDirectory);
    }

} 
```

输出

Terminal

```java
 /home/mkyong/projects/core-java/java-io
/home/mkyong/projects/core-java/java-io
/home/mkyong/projects/core-java/java-io
/home/mkyong/projects/core-java/java-io 
```

通常，我们使用`System.getProperty("user.dir")`来获取当前工作目录。

## 2.JAR 文件的工作目录？

不要使用`System.getProperty("user.dir")`、`File`或`Path`来访问`JAR`文件中的文件，这不会起作用。相反，我们应该使用`getClassLoader().getResourceAsStream()`。

```java
 // get a file from the resources folder, root of classpath in JAR
private InputStream getFileFromResourceAsStream(String fileName) {

    // The class loader that loaded the class
    ClassLoader classLoader = getClass().getClassLoader();
    InputStream inputStream = classLoader.getResourceAsStream(fileName);

    // the stream holding the file content
    if (inputStream == null) {
        throw new IllegalArgumentException("file not found! " + fileName);
    } else {
        return inputStream;
    }

} 
```

**注**
以上代码摘自本[从](/web/20220619003337/https://mkyong.com/java/java-read-a-file-from-resources-folder/)资源文件夹中读取的一个文件。请参考示例 2 来访问 JAR 文件中的文件。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003337/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Java–从资源文件夹中读取文件](/web/20220619003337/https://mkyong.com/java/java-read-a-file-from-resources-folder/)
*   [文件 JavaDoc](http://web.archive.org/web/20220619003337/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)
*   [路径 JavaDoc](http://web.archive.org/web/20220619003337/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Path.html)
*   [如何显示所有系统属性](http://web.archive.org/web/20220619003337/https://mkyong.com/java/how-to-list-all-system-properties-key-and-value-in-java/)
*   [维基百科–工作目录](http://web.archive.org/web/20220619003337/https://en.wikipedia.org/wiki/Working_directory)

<input type="hidden" id="mkyong-current-postId" value="2847">