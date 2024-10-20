# 如何在 Java 中重命名或移动文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-rename-file-in-java/>

在 Java 中，我们可以使用 NIO `Files.move(source, target)`来重命名或移动文件。

```java
 import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

  //...

  Path source = Paths.get("/home/mkyong/newfolder/test1.txt");
  Path target = Paths.get("/home/mkyong/newfolder/test2.txt");

  try{

    Files.move(source, target);

  } catch (IOException e) {
    e.printStackTrace();
  } 
```

## 1.重命名同一目录中的文件。

1.1 此示例重命名同一目录中的一个文件，保留相同的文件名。

*   从此处重命名文件`/home/mkyong/hello.txt`
*   到此`/home/mkyong/newName.txt`

```java
 Path source = Paths.get("/home/mkyong/hello.txt");

  try{

    // rename a file in the same directory
    Files.move(source, source.resolveSibling("newName.txt"));

  } catch (IOException e) {
    e.printStackTrace();
  } 
```

1.2 如果目标文件存在，`Files.move`抛出`FileAlreadyExistsException`。

```java
 java.nio.file.FileAlreadyExistsException: /home/mkyong/newName.txt
	at java.base/sun.nio.fs.UnixCopyFile.move(UnixCopyFile.java:450)
	at java.base/sun.nio.fs.UnixFileSystemProvider.move(UnixFileSystemProvider.java:267)
	at java.base/java.nio.file.Files.move(Files.java:1421)
	at com.mkyong.io.file.FileRename.main(FileRename.java:26) 
```

1.3 如果指定了`REPLACE_EXISTING`选项，并且目标文件存在，则`Files.move`将替换它。

```java
 import java.nio.file.StandardCopyOption;

  Files.move(source, source.resolveSibling("newName.txt"),
              StandardCopyOption.REPLACE_EXISTING); 
```

## 2.将文件移动到新目录。

2.1 本示例将一个文件移动到一个新目录，并保留相同的文件名。如果目标文件存在，替换它。

*   从此处移动文件`/home/mkyong/hello.txt`
*   到此`/home/mkyong/newfolder/hello.txt`

```java
 Path source = Paths.get("/home/mkyong/hello.txt");

  Path newDir = Paths.get("/home/mkyong/newfolder/");

  //create the target directories, if directory exits, no effect
  Files.createDirectories(newDir);

  Files.move(source, newDir.resolve(source.getFileName()),
              StandardCopyOption.REPLACE_EXISTING); 
```

2.2 如果目标目录不存在，`Files.move`抛出`NoSuchFileException`。

```java
 java.nio.file.NoSuchFileException: /home/mkyong/hello.txt -> /home/mkyong/newfolder/hello2.txt
  	at java.base/sun.nio.fs.UnixException.translateToIOException(UnixException.java:92)
  	at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:111)
  	at java.base/sun.nio.fs.UnixCopyFile.move(UnixCopyFile.java:478)
  	at java.base/sun.nio.fs.UnixFileSystemProvider.move(UnixFileSystemProvider.java:267)
  	at java.base/java.nio.file.Files.move(Files.java:1421)
  	at com.mkyong.io.file.FileRename.main(FileRename.java:23) 
```

## 3.移动文件–Apache Commons IO

3.1 Apache`FileUtils.moveFile`使用“复制和删除”机制来重命名或移动文件。此外，它做了大量的检查，确保抛出正确的异常，这是一个可靠的解决方案。

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

    File source = new File("/home/mkyong/hello.txt");
    File target = new File("/home/mkyong/newfolder/hello2.txt");

    try {

        FileUtils.moveFile(source, target);

    } catch (IOException e) {
        e.printStackTrace();
    } 
```

3.2 如果目标文件存在，抛出`FileExistsException`。

```java
 org.apache.commons.io.FileExistsException: Destination '/home/mkyong/newfolder/hello2.txt' already exists
	at org.apache.commons.io.FileUtils.moveFile(FileUtils.java:2012)
	at com.mkyong.io.file.FileRename.main(FileRename.java:39) 
```

3.3 如果目标目录不存在，请创建它。

## 4.File.renameTo(旧 IO)

遗留 IO `File.renameTo()`不可靠，不推荐使用，阅读 [api 文档](http://web.archive.org/web/20220619003339/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html#renameTo(java.io.File))。

如果`File.renameTo()`未能重命名或移动文件，它将返回 false，不会抛出异常，通常情况下，我们不知道哪里出错了。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003339/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [移动文件或目录](http://web.archive.org/web/20220619003339/https://docs.oracle.com/javase/tutorial/essential/io/move.html)
*   [NIO 文件 JavaDoc](http://web.archive.org/web/20220619003339/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [遗留文件 JavaDoc](http://web.archive.org/web/20220619003339/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html#renameTo(java.io.File))
*   [Apache FileUtils JavaDoc](http://web.archive.org/web/20220619003339/https://commons.apache.org/proper/commons-io/javadocs/api-2.7/org/apache/commons/io/FileUtils.html)

<input type="hidden" id="mkyong-current-postId" value="5464">