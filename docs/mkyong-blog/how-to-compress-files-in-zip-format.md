# 如何在 Java 中创建 Zip 文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-compress-files-in-zip-format/>

本文展示了压缩单个文件和整个目录(包括子文件和子目录)的几个例子。

1.  压缩文件-`java.util.zip`
2.  压缩文件-`Files.copy`到`Zip FileSystems`
3.  按需压缩文件(不写入磁盘)
4.  压缩文件夹-文件树和`java.util.zip`
5.  压缩文件夹-文件树和`Files.copy`到`Zip FileSystems`
6.  `zipj4`图书馆

Java 7 引入了 [Zip 文件系统提供者](http://web.archive.org/web/20220817155755/https://docs.oracle.com/javase/8/docs/technotes/guides/io/fsp/zipfilesystemprovider.html)，结合`Files.copy`，我们可以很容易地将文件属性复制到 Zip 文件中(见例 4)。

## 1.压缩单个文件–Java . util . zip

1.1 这个 Java 示例使用`java.util.zip.ZipOutputStream`压缩单个文件。

ZipFileExample1.java

```java
 package com.mkyong.io.howto;

import java.io.*;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

public class ZipFileExample1 {

    public static void main(String[] args) {

        Path source = Paths.get("/home/mkyong/test/Test.java");
        String zipFileName = "example.zip";

        try {

            ZipExample.zipSingleFile(source, zipFileName);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    // Zip a single file
    public static void zipSingleFile(Path source, String zipFileName)
        throws IOException {

        if (!Files.isRegularFile(source)) {
            System.err.println("Please provide a file.");
            return;
        }

        try (
            ZipOutputStream zos = new ZipOutputStream(
                new FileOutputStream(zipFileName));
            FileInputStream fis = new FileInputStream(source.toFile());
        ) {

            ZipEntry zipEntry = new ZipEntry(source.getFileName().toString());
            zos.putNextEntry(zipEntry);

            byte[] buffer = new byte[1024];
            int len;
            while ((len = fis.read(buffer)) > 0) {
                zos.write(buffer, 0, len);
            }
            zos.closeEntry();
        }

    }
} 
```

输出

Terminal

```java
 $ unzip -l example.zip
Archive:  example.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
       32  2020-08-06 16:19   Test.java
---------                     -------
       32                     1 file

$ unzip example.zip
Archive:  example.zip
  inflating: Test.java 
```

## 2.压缩单个文件–文件系统

2.1 这个例子使用 Java 7 NIO `FileSystems.newFileSystem`创建一个 zip 文件，并使用`Files.copy`将文件复制到 zip 路径中。

ZipFileExample2.java

```java
 package com.mkyong.io.howto;

import java.io.*;
import java.net.URI;
import java.nio.file.*;
import java.util.HashMap;
import java.util.Map;

public class ZipFileExample2 {

    public static void main(String[] args) {

        Path source = Paths.get("/home/mkyong/test/Test.java");
        String zipFileName = "example.zip";

        try {

            ZipExample.zipSingleFileNio(source, zipFileName);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    // Zip a single file
    public static void zipSingleFileNio(Path source, String zipFileName)
        throws IOException {

        if (!Files.isRegularFile(source)) {
            System.err.println("Please provide a file.");
            return;
        }

        Map<String, String> env = new HashMap<>();
        // Create the zip file if it doesn't exist
        env.put("create", "true");

        URI uri = URI.create("jar:file:/home/mkyong/" + zipFileName);

        try (FileSystem zipfs = FileSystems.newFileSystem(uri, env)) {
            Path pathInZipfile = zipfs.getPath(source.getFileName().toString());

            // Copy a file into the zip file path
            Files.copy(source, pathInZipfile, StandardCopyOption.REPLACE_EXISTING);
        }

    }

} 
```

## 3.按需压缩单个文件

这个例子使用`ByteArrayInputStream`直接按需创建一些字节并保存到 zip 文件中，而不保存或写入数据到本地文件系统中。

```java
 // create a file on demand (without save locally) and add to zip
    public static void zipFileWithoutSaveLocal(String zipFileName) throws IOException {

        String data = "Test data \n123\n456";
        String fileNameInZip = "abc.txt";

        try (ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(zipFileName))) {

            ZipEntry zipEntry = new ZipEntry(fileNameInZip);
            zos.putNextEntry(zipEntry);

            ByteArrayInputStream bais = new ByteArrayInputStream(data.getBytes());
            // one line, able to handle large size?
            //zos.write(bais.readAllBytes());

            // play safe
            byte[] buffer = new byte[1024];
            int len;
            while ((len = bais.read(buffer)) > 0) {
                zos.write(buffer, 0, len);
            }

            zos.closeEntry();
        }

    } 
```

输出

Terminal

```java
 $ unzip -l example.zip

Archive:  example.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
       18  2020-08-11 18:44   abc.txt
---------                     -------
       18                     1 file 
```

## 4.压缩文件夹或目录–Java . util . zip

4.1 查看包含一些子文件和子目录的目录。

Terminal

```java
 $ tree /home/mkyong/test
test
├── data
│   └── db.debug.conf
├── README.md
├── test-a1.log
├── test-a2.log
├── test-b
│   ├── test-b1.txt
│   ├── test-b2.txt
│   ├── test-c
│   │   ├── test-c1.log
│   │   └── test-c2.log
│   └── test-d
│       ├── test-d1.log
│       └── test-d2.log
└── Test.java 
```

4.2 这个 Java 示例使用`FileVisitor`遍历文件树，使用`ZipOutputStream`手动压缩所有内容，包括子文件和子目录，但忽略符号链接和文件属性。

ZipDirectoryExample1.java

```java
 package com.mkyong.io.howto;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributes;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

public class ZipDirectoryExample {

    public static void main(String[] args) {

        Path source = Paths.get("/home/mkyong/test/");

        if (!Files.isDirectory(source)) {
            System.out.println("Please provide a folder.");
            return;
        }

        try {

            ZipDirectoryExample.zipFolder(source);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    // zip a directory, including sub files and sub directories
    public static void zipFolder(Path source) throws IOException {

        // get folder name as zip file name
        String zipFileName = source.getFileName().toString() + ".zip";

        try (
                ZipOutputStream zos = new ZipOutputStream(
                        new FileOutputStream(zipFileName))
        ) {

            Files.walkFileTree(source, new SimpleFileVisitor<>() {
                @Override
                public FileVisitResult visitFile(Path file,
                    BasicFileAttributes attributes) {

                    // only copy files, no symbolic links
                    if (attributes.isSymbolicLink()) {
                        return FileVisitResult.CONTINUE;
                    }

                    try (FileInputStream fis = new FileInputStream(file.toFile())) {

                        Path targetFile = source.relativize(file);
                        zos.putNextEntry(new ZipEntry(targetFile.toString()));

                        byte[] buffer = new byte[1024];
                        int len;
                        while ((len = fis.read(buffer)) > 0) {
                            zos.write(buffer, 0, len);
                        }

                        // if large file, throws out of memory
                        //byte[] bytes = Files.readAllBytes(file);
                        //zos.write(bytes, 0, bytes.length);

                        zos.closeEntry();

                        System.out.printf("Zip file : %s%n", file);

                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                    return FileVisitResult.CONTINUE;
                }

                @Override
                public FileVisitResult visitFileFailed(Path file, IOException exc) {
                    System.err.printf("Unable to zip : %s%n%s%n", file, exc);
                    return FileVisitResult.CONTINUE;
                }
            });

        }

    }

} 
```

输出

Terminal

```java
 Zip file : /home/mkyong/test/test-a2.log
Zip file : /home/mkyong/test/test-a1.log
Zip file : /home/mkyong/test/data/db.debug.conf
Zip file : /home/mkyong/test/README.md
Zip file : /home/mkyong/test/Test.java
Zip file : /home/mkyong/test/test-b/test-b1.txt
Zip file : /home/mkyong/test/test-b/test-c/test-c2.log
Zip file : /home/mkyong/test/test-b/test-c/test-c1.log
Zip file : /home/mkyong/test/test-b/test-b2.txt
Zip file : /home/mkyong/test/test-b/test-d/test-d2.log
Zip file : /home/mkyong/test/test-b/test-d/test-d1.log

Done 
```

上面的例子在当前的工作目录下创建了 zip 文件，我们没有复制文件的属性(查看文件创建的日期和时间)。

Terminal

```java
 $ unzip -l test.zip

Archive:  test.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2020-08-06 18:49   test-a2.log
        0  2020-08-06 18:49   test-a1.log
       14  2020-08-06 18:49   data/db.debug.conf
       42  2020-08-06 18:49   README.md
       32  2020-08-06 18:49   Test.java
        0  2020-08-06 18:49   test-b/test-b1.txt
        0  2020-08-06 18:49   test-b/test-c/test-c2.log
        0  2020-08-06 18:49   test-b/test-c/test-c1.log
        0  2020-08-06 18:49   test-b/test-b2.txt
        0  2020-08-06 18:49   test-b/test-d/test-d2.log
        0  2020-08-06 18:49   test-b/test-d/test-d1.log
---------                     -------
       88                     11 files 
```

## 5.压缩文件夹或目录——文件系统

5.1 本例使用相同的`FileVisitor`遍历文件树。不过，这次我们使用`FileSystems` URI 创建 zip 文件，使用`Files.copy`将文件复制到 zip 路径，包括**文件属性**，但是忽略符号链接。

ZipDirectoryExample2.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.io.IOException;
import java.net.URI;
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributes;
import java.util.HashMap;
import java.util.Map;

public class ZipDirectoryExample {

    public static void main(String[] args) {

        Path source = Paths.get("/home/mkyong/test/");

        if (!Files.isDirectory(source)) {
            System.out.println("Please provide a folder.");
            return;
        }

        try {

            ZipDirectoryExample.zipFolderNio(source);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    public static void zipFolderNio(Path source) throws IOException {

        // get current working directory
        String currentPath = System.getProperty("user.dir") + File.separator;

        // get folder name as zip file name
        // can be other extension, .foo .bar .whatever
        String zipFileName = source.getFileName().toString() + ".zip";
        URI uri = URI.create("jar:file:" + currentPath + zipFileName);

        Files.walkFileTree(source, new SimpleFileVisitor<>() {
            @Override
            public FileVisitResult visitFile(Path file,
                BasicFileAttributes attributes) {

                // Copying of symbolic links not supported
                if (attributes.isSymbolicLink()) {
                    return FileVisitResult.CONTINUE;
                }

                Map<String, String> env = new HashMap<>();
                env.put("create", "true");

                try (FileSystem zipfs = FileSystems.newFileSystem(uri, env)) {

                    Path targetFile = source.relativize(file);
                    Path pathInZipfile = zipfs.getPath(targetFile.toString());

                    // NoSuchFileException, need create parent directories in zip path
                    if (pathInZipfile.getParent() != null) {
                        Files.createDirectories(pathInZipfile.getParent());
                    }

                    // copy file attributes
                    CopyOption[] options = {
                            StandardCopyOption.REPLACE_EXISTING,
                            StandardCopyOption.COPY_ATTRIBUTES,
                            LinkOption.NOFOLLOW_LINKS
                    };
                    // Copy a file into the zip file path
                    Files.copy(file, pathInZipfile, options);

                } catch (IOException e) {
                    e.printStackTrace();
                }

                return FileVisitResult.CONTINUE;
            }

            @Override
            public FileVisitResult visitFileFailed(Path file, IOException exc) {
                System.err.printf("Unable to zip : %s%n%s%n", file, exc);
                return FileVisitResult.CONTINUE;
            }

        });

    }

} 
```

输出

Terminal

```java
 $ unzip -l test.zip
Archive:  test.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2020-07-27 15:10   test-a2.log
        0  2020-07-23 14:55   test-a1.log
        0  2020-08-06 18:57   data/
       14  2020-08-04 14:07   data/db.debug.conf
       42  2020-08-05 19:04   README.md
       32  2020-08-05 19:04   Test.java
        0  2020-08-06 18:57   test-b/
        0  2020-07-24 15:49   test-b/test-b1.txt
        0  2020-08-06 18:57   test-b/test-c/
        0  2020-07-27 15:11   test-b/test-c/test-c2.log
        0  2020-07-27 15:11   test-b/test-c/test-c1.log
        0  2020-07-27 15:10   test-b/test-b2.txt
        0  2020-08-06 18:57   test-b/test-d/
        0  2020-07-27 15:11   test-b/test-d/test-d2.log
        0  2020-07-27 15:11   test-b/test-d/test-d1.log
---------                     -------
       88                     15 files

# unzip to an external folder abc
$ unzip test.zip -d abc

Archive:  test.zip
 inflating: abc/test-a2.log         
 inflating: abc/test-a1.log         
  creating: abc/data/
 inflating: abc/data/db.debug.conf  
 inflating: abc/README.md           
 inflating: abc/Test.java           
  creating: abc/test-b/
 inflating: abc/test-b/test-b1.txt  
  creating: abc/test-b/test-c/
 inflating: abc/test-b/test-c/test-c2.log  
 inflating: abc/test-b/test-c/test-c1.log  
 inflating: abc/test-b/test-b2.txt  
  creating: abc/test-b/test-d/
 inflating: abc/test-b/test-d/test-d2.log  
 inflating: abc/test-b/test-d/test-d1.log  

$ tree abc
abc
├── data
│   └── db.debug.conf
├── README.md
├── test-a1.log
├── test-a2.log
├── test-b
│   ├── test-b1.txt
│   ├── test-b2.txt
│   ├── test-c
│   │   ├── test-c1.log
│   │   └── test-c2.log
│   └── test-d
│       ├── test-d1.log
│       └── test-d2.log
└── Test.java 
```

## 6.Zip 文件–zip4j

`zip4j`是 Java 中流行的 zip 库；它有许多先进的功能，如密码保护的压缩文件，分裂压缩文件，AES 加密等。请访问 [zip4j github](http://web.archive.org/web/20220817155755/https://github.com/srikanth-lingala/zip4j) 获取更多文档和用法。

以下是压缩文件和文件夹的一些常见用法。

```java
 import net.lingala.zip4j.ZipFile;

//...
  public static void zip4j() throws IOException {

      // zip file with a single file
      new ZipFile("filename.zip").addFile("file.txt");

      // zip file with multiple files
      List<File> files = Arrays.asList(
              new File("file1.txt"), new File("file2.txt"));
      new ZipFile("filename.zip").addFiles(files);

      // zip file with a folder
      new ZipFile("filename.zip").addFolder(new File("/home/mkyong/folder"));

  } 
```

**延伸阅读**
[如何用 Java 解压 zip 文件](/web/20220817155755/https://mkyong.com/java/how-to-decompress-files-from-a-zip-file/)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220817155755/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Zip 文件系统提供商](http://web.archive.org/web/20220817155755/https://docs.oracle.com/javase/8/docs/technotes/guides/io/fsp/zipfilesystemprovider.html)
*   [ZIP 文件格式规范](http://web.archive.org/web/20220817155755/https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)
*   [FileVisitor JavaDoc](http://web.archive.org/web/20220817155755/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/FileVisitor.html)
*   [如何在 Java 中获取当前工作目录](/web/20220817155755/https://mkyong.com/java/how-to-get-the-current-working-directory-in-java/)
*   [ZipOutputStream JavaDoc](http://web.archive.org/web/20220817155755/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/ZipOutputStream.html)
*   [zip4j github](http://web.archive.org/web/20220817155755/https://github.com/srikanth-lingala/zip4j)
*   [如何用 Java 解压 zip 文件](/web/20220817155755/https://mkyong.com/java/how-to-decompress-files-from-a-zip-file/)

<input type="hidden" id="mkyong-current-postId" value="3001">