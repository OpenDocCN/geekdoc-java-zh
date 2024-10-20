# Java——解压缩 Gzip 文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-decompress-file-from-gzip-file/>

在上一篇文章中，我们展示了如何用 Gzip 格式压缩文件。这篇文章展示了如何解压缩一个 Gzip 文件。

## 1.解压缩 Gzip 文件–GZIPInputStream

1.1 这个例子将一个 Gzip 文件`sitemap.xml.gz`解压回它的原始文件`sitemap.xml`。我们将 [GZIPInputStream](http://web.archive.org/web/20220629161538/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/GZIPInputStream.html) 复制到一个`FileOutputStream`中来解压一个 Gzip 文件。

GZipExample.java

```java
 package com.mkyong.io.howto.compress;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.zip.GZIPInputStream;

public class GZipExample {

    public static void main(String[] args) {

        Path source = Paths.get("/home/mkyong/test/sitemap.xml.gz");
        Path target = Paths.get("/home/mkyong/test/sitemap.xml");

        if (Files.notExists(source)) {
            System.err.printf("The path %s doesn't exist!", source);
            return;
        }

        try {

            GZipExample.decompressGzip(source, target);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static void decompressGzip(Path source, Path target) throws IOException {

        try (GZIPInputStream gis = new GZIPInputStream(
                                      new FileInputStream(source.toFile()));
             FileOutputStream fos = new FileOutputStream(target.toFile())) {

            // copy GZIPInputStream to FileOutputStream
            byte[] buffer = new byte[1024];
            int len;
            while ((len = gis.read(buffer)) > 0) {
                fos.write(buffer, 0, len);
            }

        }

    }

} 
```

## 2.再次解压缩 Gzip 文件。

2.1 该实例类似于实例 1；相反，我们使用 NIO `File.copy`来复制文件。

```java
 import java.nio.file.Files;
import java.util.zip.GZIPInputStream;

  //...
  public static void decompressGzipNio(Path source, Path target) throws IOException {

      try (GZIPInputStream gis = new GZIPInputStream(
                                    new FileInputStream(source.toFile()))) {

          Files.copy(gis, target);
      }

  } 
```

## 3.将 Gzip 文件解压缩为字节数组

3.1 这个例子将一个 Gzip 文件直接解压到一个`byte[]`文件中，而不是保存到本地文件中。

GZipExample3.java

```java
 package com.mkyong.io.howto.compress;

import java.io.ByteArrayOutputStream;
import java.io.FileInputStream;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.zip.GZIPInputStream;

public class GZipExample3 {

    public static void main(String[] args) {

        // decompress
        Path source = Paths.get("/home/mkyong/test/sitemap.xml.gz");

        if (Files.notExists(source)) {
            System.err.printf("The path %s doesn't exist!", source);
            return;
        }

        try {

            byte[] bytes = GZipExample.decompressGzipToBytes(source);
            // do task for the byte[]

            //convert byte[] to string for display
            System.out.println(new String(bytes, StandardCharsets.UTF_8));

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // decompress a Gzip file into a byte arrays
    public static byte[] decompressGzipToBytes(Path source) throws IOException {

        ByteArrayOutputStream output = new ByteArrayOutputStream();

        try (GZIPInputStream gis = new GZIPInputStream(
                                      new FileInputStream(source.toFile()))) {

            // copy GZIPInputStream to ByteArrayOutputStream
            byte[] buffer = new byte[1024];
            int len;
            while ((len = gis.read(buffer)) > 0) {
                output.write(buffer, 0, len);
            }

        }

        return output.toByteArray();

    }

} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629161538/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [GZIPInputStream JavaDoc](http://web.archive.org/web/20220629161538/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/GZIPInputStream.html)
*   [Java–以 Gzip 格式压缩文件](/web/20220629161538/https://mkyong.com/java/how-to-compress-a-file-in-gzip-format/)。
*   [维基百科–GZip](http://web.archive.org/web/20220629161538/https://en.wikipedia.org/wiki/Gzip)
*   [gzip 站点](http://web.archive.org/web/20220629161538/http://www.gzip.org/)
*   [GNU Gzip](http://web.archive.org/web/20220629161538/https://www.gnu.org/software/gzip/)

<input type="hidden" id="mkyong-current-postId" value="3026">