# Java——以 Gzip 格式压缩文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-compress-a-file-in-gzip-format/>

本文展示了如何使用 Java 压缩 Gzip 格式的文件。

**1。什么是 Gzip？**
[GZip](http://web.archive.org/web/20220619003338/https://en.wikipedia.org/wiki/Gzip) 是 Unix 或 Linux 系统上的标准文件压缩工具，一般都有后缀`.gz`。

**2。为什么我们需要用 Gzip 压缩文件？**
答:文件更小，节省磁盘空间。

**3。tar.gz 怎么样？**
`Gzip`压缩单个文件，而`Tar`则是将文件收集成一个归档文件，访问本文即可在 Java 中[创建 tar.gz](/web/20220619003338/https://mkyong.com/java/how-to-create-tar-gz-in-java/)

## 1.压缩 Gzip–GZIPOutputStream 中的文件

我们将`FileInputStream`复制到 [GZIPOutputStream](http://web.archive.org/web/20220619003338/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/GZIPOutputStream.html) 中，以 Gzip 格式压缩文件。

1.1 以下示例将文件`sitemap.xml`压缩成 Gzip 文件`sitemap.xml.gz`。

GZipExample.java

```java
 package com.mkyong.io.howto.compress;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.zip.GZIPOutputStream;

public class GZipExample {

    public static void main(String[] args) {

        // compress a file
        Path source = Paths.get("/home/mkyong/test/sitemap.xml");
        Path target = Paths.get("/home/mkyong/test/sitemap.xml.gz");

        if (Files.notExists(source)) {
            System.err.printf("The path %s doesn't exist!", source);
            return;
        }

        try {

            GZipExample.compressGzip(source, target);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // copy file (FileInputStream) to GZIPOutputStream
    public static void compressGzip(Path source, Path target) throws IOException {

        try (GZIPOutputStream gos = new GZIPOutputStream(
                                      new FileOutputStream(target.toFile()));
             FileInputStream fis = new FileInputStream(source.toFile())) {

            // copy file
            byte[] buffer = new byte[1024];
            int len;
            while ((len = fis.read(buffer)) > 0) {
                gos.write(buffer, 0, len);
            }

        }

    }

} 
```

输出，XML 文件以 Gzip 格式压缩，文件大小从 388k 降到 40k。

Terminal

```java
 $ ls -lsah sitemap.*
388K -rw-rw-r-- 1 mkyong mkyong 388K Ogos 12 14:02 sitemap.xml
 40K -rw-rw-r-- 1 mkyong mkyong  40K Ogos 12 14:06 sitemap.xml.gz

$ gzip -l sitemap.xml.gz
       compressed        uncompressed  ratio uncompressed_name
            40154              396719  89.9% sitemap.xml 
```

1.2 本例与例 1.1 相似。相反，我们使用 NIO `Files.copy`将一个`Path`直接复制到`GZIPOutputStream`。

```java
 public static void compressGzipNio(Path source, Path target) throws IOException {

      try (GZIPOutputStream gos = new GZIPOutputStream(
                                    new FileOutputStream(target.toFile()))) {

          Files.copy(source, gos);

      }

  } 
```

## 2.将字符串压缩到 Gzip

2.1 这个例子将一个字符串压缩成一个 Gzip 文件。

```java
 public static void compressStringToGzip(String data, Path target) throws IOException {

      try (GZIPOutputStream gos = new GZIPOutputStream(
                                    new FileOutputStream(target.toFile()))) {

          gos.write(data.getBytes(StandardCharsets.UTF_8));

      }

  } 
```

**延伸阅读**
[Java——如何解压一个 Gzip 文件](/web/20220619003338/https://mkyong.com/java/how-to-decompress-file-from-gzip-file/)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003338/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [GZIPOutputStream JavaDoc](http://web.archive.org/web/20220619003338/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/GZIPOutputStream.html)
*   [维基百科–GZip](http://web.archive.org/web/20220619003338/https://en.wikipedia.org/wiki/Gzip)
*   [gzip 站点](http://web.archive.org/web/20220619003338/http://www.gzip.org/)
*   [GNU Gzip](http://web.archive.org/web/20220619003338/https://www.gnu.org/software/gzip/)
*   [Gzip vs Zip](http://web.archive.org/web/20220619003338/https://nixcp.com/gzip-vs-zip-differences/)
*   [Java——如何解压 GZIP 文件](/web/20220619003338/https://mkyong.com/java/how-to-decompress-file-from-gzip-file/)
*   [用 Java 创建 tar.gz](/web/20220619003338/https://mkyong.com/java/how-to-create-tar-gz-in-java/)

<input type="hidden" id="mkyong-current-postId" value="3015">