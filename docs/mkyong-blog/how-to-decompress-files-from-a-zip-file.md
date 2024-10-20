# 如何用 Java 解压一个 zip 文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-decompress-files-from-a-zip-file/>

这篇文章展示了如何使用`ZipInputStream`和`zip4j`库在 Java 中解压一个 zip 文件。

要手动解压缩文件，记得添加对 [zip slip 漏洞](http://web.archive.org/web/20220607032904/https://snyk.io/research/zip-slip-vulnerability)的验证。

```java
 Path targetDirResolved = targetDir.resolve(zipEntry.getName());

  // make sure normalized file still has targetDir as its prefix
  Path normalizePath = targetDirResolved.normalize();

  if (!normalizePath.startsWith(targetDir)) {
      // may be zip slip, better stop and throws exception
      throw new IOException("Bad zip entry: " + zipEntry.getName());
  } 
```

## 1.压缩文件

下面这个 zip 文件结构就是本文创建的——[用 Java 创建 zip 文件](/web/20220607032904/https://mkyong.com/java/how-to-compress-files-in-zip-format/)。稍后我们将展示如何将它解压到一个新的文件夹中。

1.1 zip 文件只包含文件。

/home/mkyong/zip/test.zip

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

1.2 zip 文件包含文件夹和文件。

/home/mkyong/zip/test.zip

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
```

## 2\. Unzip file – ZipInputStream

要解压缩一个 zip 文件，我们使用`ZipInputStream`来读取 zip 文件，并将文件从 zip 文件复制到一个新的文件夹中(zip 文件之外)。

下面这个例子将一个 zip 文件`/home/mkyong/zip/test.zip`解压到一个文件夹`/home/mkyong/zip/`，也提供了一个验证来防止 [zip slip 漏洞](http://web.archive.org/web/20220607032904/https://snyk.io/research/zip-slip-vulnerability)。

ZipFileUnZipExample.java

```java
 package com.mkyong.io.howto;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;

public class ZipFileUnZipExample {

    public static void main(String[] args) {

        Path source = Paths.get("/home/mkyong/zip/test.zip");
        Path target = Paths.get("/home/mkyong/zip/");

        try {

            unzipFolder(source, target);
            System.out.println("Done");

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static void unzipFolder(Path source, Path target) throws IOException {

        try (ZipInputStream zis = new ZipInputStream(new FileInputStream(source.toFile()))) {

            // list files in zip
            ZipEntry zipEntry = zis.getNextEntry();

            while (zipEntry != null) {

                boolean isDirectory = false;
                // example 1.1
                // some zip stored files and folders separately
                // e.g data/
                //     data/folder/
                //     data/folder/file.txt
                if (zipEntry.getName().endsWith(File.separator)) {
                    isDirectory = true;
                }

                Path newPath = zipSlipProtect(zipEntry, target);

                if (isDirectory) {
                    Files.createDirectories(newPath);
                } else {

                    // example 1.2
                    // some zip stored file path only, need create parent directories
                    // e.g data/folder/file.txt
                    if (newPath.getParent() != null) {
                        if (Files.notExists(newPath.getParent())) {
                            Files.createDirectories(newPath.getParent());
                        }
                    }

                    // copy files, nio
                    Files.copy(zis, newPath, StandardCopyOption.REPLACE_EXISTING);

                    // copy files, classic
                    /*try (FileOutputStream fos = new FileOutputStream(newPath.toFile())) {
                        byte[] buffer = new byte[1024];
                        int len;
                        while ((len = zis.read(buffer)) > 0) {
                            fos.write(buffer, 0, len);
                        }
                    }*/
                }

                zipEntry = zis.getNextEntry();

            }
            zis.closeEntry();

        }

    }

    // protect zip slip attack
    public static Path zipSlipProtect(ZipEntry zipEntry, Path targetDir)
        throws IOException {

        // test zip slip vulnerability
        // Path targetDirResolved = targetDir.resolve("../../" + zipEntry.getName());

        Path targetDirResolved = targetDir.resolve(zipEntry.getName());

        // make sure normalized file still has targetDir as its prefix
        // else throws exception
        Path normalizePath = targetDirResolved.normalize();
        if (!normalizePath.startsWith(targetDir)) {
            throw new IOException("Bad zip entry: " + zipEntry.getName());
        }

        return normalizePath;
    }

} 
```

输出

Terminal

```java
 $ pwd
/home/mkyong/zip/

$ tree
.
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
├── Test.java 
```

## 3.解压缩文件–zip4j

这个例子使用了 [zip4j](http://web.archive.org/web/20220607032904/https://github.com/srikanth-lingala/zip4j) 库来解压一个 zip 文件。

pom.xml

```java
 <dependency>
      <groupId>net.lingala.zip4j</groupId>
      <artifactId>zip4j</artifactId>
      <version>2.6.1</version>
  </dependency> 
```

```java
 // it takes `File` as arguments
  public static void unzipFolderZip4j(Path source, Path target)
        throws IOException {

        new ZipFile(source.toFile())
                .extractAll(target.toString());

  } 
```

## 4.ZipException:无效的条目大小

如果我们遇到下面的`invalid entry size`异常，这意味着 zip 文件在复制、传输或创建过程中被破坏。没有办法修复损坏的文件大小，再次获得一个新的 zip 文件。

Terminal

```java
 java.util.zip.ZipException: invalid entry size (expected 0 but got 1282 bytes)
	at java.base/java.util.zip.ZipInputStream.readEnd(ZipInputStream.java:398)
	at java.base/java.util.zip.ZipInputStream.read(ZipInputStream.java:197)
	at java.base/java.io.FilterInputStream.read(FilterInputStream.java:107)
	at com.mkyong.io.howto.ZipFileUnZipExample.unzipFolder(ZipFileUnZipExample.java:63)
	at com.mkyong.io.howto.ZipFileUnZipExample.main(ZipFileUnZipExample.java:22) 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220607032904/https://github.com/mkyong/core-java)

$ cd java-io/howto

## 参考

*   [维基百科–邮编](http://web.archive.org/web/20220607032904/https://en.wikipedia.org/wiki/Zip_(file_format))
*   [用 Java 创建一个 Zip 文件](/web/20220607032904/https://mkyong.com/java/how-to-compress-files-in-zip-format/)
*   [在档案提取期间任意文件写入(“Zip Slip”)](http://web.archive.org/web/20220607032904/https://help.semmle.com/wiki/pages/viewpage.action?pageId=29393405)
*   [zip4j 库](http://web.archive.org/web/20220607032904/https://github.com/srikanth-lingala/zip4j)
*   [Zip 文件系统提供商](http://web.archive.org/web/20220607032904/https://docs.oracle.com/javase/8/docs/technotes/guides/io/fsp/zipfilesystemprovider.html)
*   [ZIP 文件格式规范](http://web.archive.org/web/20220607032904/https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)
*   [ZipInputStream JavaDoc](http://web.archive.org/web/20220607032904/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/ZipInputStream.html)

<input type="hidden" id="mkyong-current-postId" value="3005">