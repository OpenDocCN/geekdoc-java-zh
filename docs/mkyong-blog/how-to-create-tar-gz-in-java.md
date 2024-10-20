# 如何用 Java 创建 tar.gz

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-create-tar-gz-in-java/>

在 Java 中，我们有`ZipOutputStream`到[创建一个 zip 文件](/web/20220629160044/https://mkyong.com/java/how-to-compress-files-in-zip-format/)，还有`GZIPOutputStream`到[使用 Gzip](/web/20220629160044/https://mkyong.com/java/how-to-compress-a-file-in-gzip-format/) 压缩一个文件，但是没有官方 API 创建一个`tar.gz`文件。

在 Java 中，我们可以使用 [Apache Commons Compress](http://web.archive.org/web/20220629160044/https://commons.apache.org/proper/commons-compress/) (仍在开发中)来创建一个`.tar.gz`文件。

pom.xml

```java
 <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-compress</artifactId>
      <version>1.20</version>
  </dependency> 
```

**注释**

1.  [tar](http://web.archive.org/web/20220629160044/https://en.wikipedia.org/wiki/Tar_(computing)) 用于将文件收集到一个归档文件中，即 tarball，通常带有后缀`.tar`
2.  [Gzip](http://web.archive.org/web/20220629160044/https://en.wikipedia.org/wiki/Gzip) 用于压缩文件以节省空间，通常带有后缀`.gz`
3.  `tar.gz`或`.tgz`的意思是将所有文件组合成一个归档文件，并用 Gzip 压缩。

下面的代码片段将创建一个`tar.gz`文件。

```java
 import org.apache.commons.compress.archivers.tar.TarArchiveEntry;
import org.apache.commons.compress.archivers.tar.TarArchiveOutputStream;
import org.apache.commons.compress.compressors.gzip.GzipCompressorOutputStream;

//...
try (OutputStream fOut = Files.newOutputStream(Paths.get("output.tar.gz"));
     BufferedOutputStream buffOut = new BufferedOutputStream(fOut);
     GzipCompressorOutputStream gzOut = new GzipCompressorOutputStream(buffOut);
     TarArchiveOutputStream tOut = new TarArchiveOutputStream(gzOut)) {

       TarArchiveEntry tarEntry = new TarArchiveEntry(file,fileName);

       tOut.putArchiveEntry(tarEntry);

       // copy file to TarArchiveOutputStream
       Files.copy(path, tOut);

       tOut.closeArchiveEntry();

       tOut.finish();

     } 
```

下面的代码片段将解压一个`.tar.gz`文件。

```java
 import org.apache.commons.compress.archivers.ArchiveEntry;
import org.apache.commons.compress.archivers.tar.TarArchiveInputStream;
import org.apache.commons.compress.compressors.gzip.GzipCompressorInputStream;

//...
try (InputStream fi = Files.newInputStream(Paths.get("input.tar.gz"));
     BufferedInputStream bi = new BufferedInputStream(fi);
     GzipCompressorInputStream gzi = new GzipCompressorInputStream(bi);
     TarArchiveInputStream ti = new TarArchiveInputStream(gzi)) {

    ArchiveEntry entry;
    while ((entry = ti.getNextEntry()) != null) {

        // create a new path, remember check zip slip attack
        Path newPath = filename(entry, targetDir);

        //checking

        // copy TarArchiveInputStream to newPath
        Files.copy(ti, newPath);

    }
} 
```

## 1.向 tar.gz 添加两个文件

这个例子展示了如何将两个文件添加到一个`tar.gz`文件中。

TarGzipExample1.java

```java
 package com.mkyong.io.howto.compress;

import org.apache.commons.compress.archivers.tar.TarArchiveEntry;
import org.apache.commons.compress.archivers.tar.TarArchiveOutputStream;
import org.apache.commons.compress.compressors.gzip.GzipCompressorOutputStream;

import java.io.BufferedOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Arrays;
import java.util.List;

public class TarGzipExample1 {

    public static void main(String[] args) {

        try {

            Path path1 = Paths.get("/home/mkyong/test/sitemap.xml");
            Path path2 = Paths.get("/home/mkyong/test/file.txt");
            Path output = Paths.get("/home/mkyong/test/output.tar.gz");

            List<Path> paths = Arrays.asList(path1, path2);
            createTarGzipFiles(paths, output);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    // tar.gz few files
    public static void createTarGzipFiles(List<Path> paths, Path output)
        throws IOException {

        try (OutputStream fOut = Files.newOutputStream(output);
             BufferedOutputStream buffOut = new BufferedOutputStream(fOut);
             GzipCompressorOutputStream gzOut = new GzipCompressorOutputStream(buffOut);
             TarArchiveOutputStream tOut = new TarArchiveOutputStream(gzOut)) {

            for (Path path : paths) {

                if (!Files.isRegularFile(path)) {
                    throw new IOException("Support only file!");
                }

                TarArchiveEntry tarEntry = new TarArchiveEntry(
                                                  path.toFile(),
                                                  path.getFileName().toString());

                tOut.putArchiveEntry(tarEntry);

                // copy file to TarArchiveOutputStream
                Files.copy(path, tOut);

                tOut.closeArchiveEntry();

            }

            tOut.finish();

        }

    }

} 
```

输出——它将`sitemap.xml`和`file.txt`添加到一个归档文件`output.tar`中，并用 Gzip 压缩，结果是一个`output.tar.gz`

Terminal

```java
 $ tar -tvf /home/mkyong/test/output.tar.gz
-rw-r--r-- 0/0          396719 2020-08-12 14:02 sitemap.xml
-rw-r--r-- 0/0              15 2020-08-11 17:56 file.txt 
```

## 2.向 tar.gz 添加一个目录

这个例子将一个目录，包括它的子文件和子目录添加到一个归档文件中，Gzip 将它压缩到一个`.tar.gz`

这个想法是用`Files.walkFileTree`遍历一个文件树，将文件一个一个的添加到`TarArchiveOutputStream`中。

TarGzipExample2.java

```java
 package com.mkyong.io.howto.compress;

import org.apache.commons.compress.archivers.tar.TarArchiveEntry;
import org.apache.commons.compress.archivers.tar.TarArchiveOutputStream;
import org.apache.commons.compress.compressors.gzip.GzipCompressorOutputStream;

import java.io.BufferedOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributes;

public class TarGzipExample2 {

    public static void main(String[] args) {

        try {

            // tar.gz a folder
            Path source = Paths.get("/home/mkyong/test");
            createTarGzipFolder(source);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    // generate .tar.gz file at the current working directory
    // tar.gz a folder
    public static void createTarGzipFolder(Path source) throws IOException {

        if (!Files.isDirectory(source)) {
            throw new IOException("Please provide a directory.");
        }

        // get folder name as zip file name
        String tarFileName = source.getFileName().toString() + ".tar.gz";

        try (OutputStream fOut = Files.newOutputStream(Paths.get(tarFileName));
             BufferedOutputStream buffOut = new BufferedOutputStream(fOut);
             GzipCompressorOutputStream gzOut = new GzipCompressorOutputStream(buffOut);
             TarArchiveOutputStream tOut = new TarArchiveOutputStream(gzOut)) {

            Files.walkFileTree(source, new SimpleFileVisitor<>() {

                @Override
                public FileVisitResult visitFile(Path file,
                                            BasicFileAttributes attributes) {

                    // only copy files, no symbolic links
                    if (attributes.isSymbolicLink()) {
                        return FileVisitResult.CONTINUE;
                    }

                    // get filename
                    Path targetFile = source.relativize(file);

                    try {
                        TarArchiveEntry tarEntry = new TarArchiveEntry(
                                file.toFile(), targetFile.toString());

                        tOut.putArchiveEntry(tarEntry);

                        Files.copy(file, tOut);

                        tOut.closeArchiveEntry();

                        System.out.printf("file : %s%n", file);

                    } catch (IOException e) {
                        System.err.printf("Unable to tar.gz : %s%n%s%n", file, e);
                    }

                    return FileVisitResult.CONTINUE;
                }

                @Override
                public FileVisitResult visitFileFailed(Path file, IOException exc) {
                    System.err.printf("Unable to tar.gz : %s%n%s%n", file, exc);
                    return FileVisitResult.CONTINUE;
                }

            });

            tOut.finish();
        }

    }

} 
```

## 3.向 tar.gz 添加字符串

这个例子将`String`添加到一个`ByteArrayInputStream`中，并将其直接放入`TarArchiveOutputStream`中。意思是创建一个文件，不保存到本地磁盘，直接放入`tar.gz`中。

```java
 public static void createTarGzipFilesOnDemand() throws IOException {

        String data1 = "Test data 1";
        String fileName1 = "111.txt";

        String data2 = "Test data 2 3 4";
        String fileName2 = "folder/222.txt";

        String outputTarGzip = "/home/mkyong/output.tar.gz";

        try (OutputStream fOut = Files.newOutputStream(Paths.get(outputTarGzip));
             BufferedOutputStream buffOut = new BufferedOutputStream(fOut);
             GzipCompressorOutputStream gzOut = new GzipCompressorOutputStream(buffOut);
             TarArchiveOutputStream tOut = new TarArchiveOutputStream(gzOut)) {

            createTarArchiveEntry(fileName1, data1, tOut);
            createTarArchiveEntry(fileName2, data2, tOut);

            tOut.finish();
        }

    }

    private static void createTarArchiveEntry(String fileName,
                                              String data,
                                              TarArchiveOutputStream tOut)
                                              throws IOException {

        byte[] dataInBytes = data.getBytes();

        // create a byte[] input stream
        ByteArrayInputStream baOut1 = new ByteArrayInputStream(dataInBytes);

        TarArchiveEntry tarEntry = new TarArchiveEntry(fileName);

        // need defined the file size, else error
        tarEntry.setSize(dataInBytes.length);
        // tarEntry.setSize(baOut1.available()); alternative

        tOut.putArchiveEntry(tarEntry);

        // copy ByteArrayInputStream to TarArchiveOutputStream
        byte[] buffer = new byte[1024];
        int len;
        while ((len = baOut1.read(buffer)) > 0) {
            tOut.write(buffer, 0, len);
        }

        tOut.closeArchiveEntry();

    } 
```

## 4.解压缩文件-tar.gz

这个例子展示了如何解压缩和提取一个`tar.gz`文件，它还检查了 [zip slip 漏洞](http://web.archive.org/web/20220629160044/https://snyk.io/research/zip-slip-vulnerability)。

TarGzipExample4.java

```java
 package com.mkyong.io.howto.compress;

import org.apache.commons.compress.archivers.ArchiveEntry;
import org.apache.commons.compress.archivers.tar.TarArchiveInputStream;
import org.apache.commons.compress.compressors.gzip.GzipCompressorInputStream;

import java.io.BufferedInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;

public class TarGzipExample4 {

    public static void main(String[] args) {

        try {

            // decompress .tar.gz
            Path source = Paths.get("/home/mkyong/test/output.tar.gz");
            Path target = Paths.get("/home/mkyong/test2");
            decompressTarGzipFile(source, target);

        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    public static void decompressTarGzipFile(Path source, Path target)
        throws IOException {

        if (Files.notExists(source)) {
            throw new IOException("File doesn't exists!");
        }

        try (InputStream fi = Files.newInputStream(source);
             BufferedInputStream bi = new BufferedInputStream(fi);
             GzipCompressorInputStream gzi = new GzipCompressorInputStream(bi);
             TarArchiveInputStream ti = new TarArchiveInputStream(gzi)) {

            ArchiveEntry entry;
            while ((entry = ti.getNextEntry()) != null) {

                // create a new path, zip slip validate
                Path newPath = zipSlipProtect(entry, target);

                if (entry.isDirectory()) {
                    Files.createDirectories(newPath);
                } else {

                    // check parent folder again
                    Path parent = newPath.getParent();
                    if (parent != null) {
                        if (Files.notExists(parent)) {
                            Files.createDirectories(parent);
                        }
                    }

                    // copy TarArchiveInputStream to Path newPath
                    Files.copy(ti, newPath, StandardCopyOption.REPLACE_EXISTING);

                }
            }
        }
    }

    private static Path zipSlipProtect(ArchiveEntry entry, Path targetDir)
        throws IOException {

        Path targetDirResolved = targetDir.resolve(entry.getName());

        // make sure normalized file still has targetDir as its prefix,
        // else throws exception
        Path normalizePath = targetDirResolved.normalize();

        if (!normalizePath.startsWith(targetDir)) {
            throw new IOException("Bad entry: " + entry.getName());
        }

        return normalizePath;
    }

} 
```

**延伸阅读**
请查看官方 [Apache Commons 压缩示例](http://web.archive.org/web/20220629160044/https://commons.apache.org/proper/commons-compress/examples.html)。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160044/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [维基百科-取](http://web.archive.org/web/20220629160044/https://en.wikipedia.org/wiki/Tar_(computing))
*   [Gzip](http://web.archive.org/web/20220629160044/https://en.wikipedia.org/wiki/Gzip)
*   [Apache Commons 压缩](http://web.archive.org/web/20220629160044/https://commons.apache.org/proper/commons-compress/)
*   [Java–创建一个 zip 文件](/web/20220629160044/https://mkyong.com/java/how-to-compress-files-in-zip-format/)
*   [Java–压缩 Gzip 中的文件](/web/20220629160044/https://mkyong.com/java/how-to-compress-a-file-in-gzip-format/)
*   [拉链滑动漏洞](http://web.archive.org/web/20220629160044/https://snyk.io/research/zip-slip-vulnerability)

<input type="hidden" id="mkyong-current-postId" value="16133">