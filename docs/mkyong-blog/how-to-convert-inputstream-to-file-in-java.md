# 如何在 Java 中将 InputStream 转换成文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-convert-inputstream-to-file-in-java/>

下面是一些将`InputStream`转换成`File`的 Java 例子。

1.  [普通 Java–文件输出流](#plain-java---fileoutputstream)
2.  [Apache common io–file utils . copyinputstreamtofile](#apache-commons-io)
3.  [Java 7–files . copy](#java-17---filescopy)
4.  [Java 9–input stream . transfer](#java-9---inputstreamtransferto)

## 1。普通 Java–文件输出流

这个例子下载了`google.com` HTML 页面，并将其作为`InputStream`返回。我们用`FileOutputStream`把`InputStream`复制成`File`，保存在某个地方。

InputStreamToFile1.java

```java
 package com.mkyong.io.howto;

import java.io.*;
import java.net.URI;

public class InputStreamToFile1 {

    /**
     * The default buffer size
     */
    public static final int DEFAULT_BUFFER_SIZE = 8192;

    public static void main(String[] args) throws IOException {

        URI u = URI.create("https://www.google.com/");
        try (InputStream inputStream = u.toURL().openStream()) {

            File file = new File("c:\\test\\google.txt");

            copyInputStreamToFile(inputStream, file);

        }

    }

    private static void copyInputStreamToFile(InputStream inputStream, File file)
            throws IOException {

        // append = false
        try (FileOutputStream outputStream = new FileOutputStream(file, false)) {
            int read;
            byte[] bytes = new byte[DEFAULT_BUFFER_SIZE];
            while ((read = inputStream.read(bytes)) != -1) {
                outputStream.write(bytes, 0, read);
            }
        }

    }

} 
```

## 2 .Apache common me〔t1〕

如果项目中有 Apache Commons IO，我们可以使用`FileUtils.copyInputStreamToFile`将`InputStream`复制到`File`中。

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

InputStreamToFile2.java

```java
 package com.mkyong.io.howto;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.net.URI;

public class InputStreamToFile2 {

    public static void main(String[] args) throws IOException {

        URI u = URI.create("https://www.google.com/");
        try (InputStream inputStream = u.toURL().openStream()) {

            File file = new File("c:\\test\\google.txt");

            // commons-io
            FileUtils.copyInputStreamToFile(inputStream, file);

        }

    }

} 
```

## 3。Java 1.7–files . copy

在 Java 1.7 中，我们可以使用`Files.copy`将`InputStream`复制到一个`Path`。

InputStreamToFile3.java

```java
 URI u = URI.create("https://www.google.com/");
  try (InputStream inputStream = u.toURL().openStream()) {

      File file = new File("c:\\test\\google.txt");

      // Java 1.7
      Files.copy(inputStream, file.toPath(), StandardCopyOption.REPLACE_EXISTING);

  } 
```

## 4 .Java 9–input stream . transfer

在 Java 9 中，我们可以使用新的 [InputStream#transferTo](http://web.archive.org/web/20220706091052/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStream.html#transferTo(java.io.OutputStream)) 将`InputStream`直接复制到`OutputStream`中。

InputStreamToFile4.java

```java
 package com.mkyong.io.howto;

import java.io.*;
import java.net.URI;

public class InputStreamToFile4 {

    public static void main(String[] args) throws IOException {

        URI u = URI.create("https://www.google.com/");
        try (InputStream inputStream = u.toURL().openStream()) {

            File file = new File("c:\\test\\google.txt");

            // Java 9
            copyInputStreamToFileJava9(inputStream, file);

        }

    }

    // Java 9
    private static void copyInputStreamToFileJava9(InputStream input, File file)
        throws IOException {

        // append = false
        try (OutputStream output = new FileOutputStream(file, false)) {
            input.transferTo(output);
        }

    }

} 
```

## 5。将文件转换为输入流

我们可以使用`FileInputStream`将一个`File`转换成一个`InputStream`。

```java
 File file = new File("d:\\download\\google.txt");
  InputStream inputStream = new FileInputStream(file); 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220706091052/https://github.com/mkyong/core-java)

$ CD Java-io/操作方法

## 参考文献

*   [FileOutputStream JavaDoc](http://web.archive.org/web/20220706091052/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileOutputStream.html)
*   [InputStream JavaDoc](http://web.archive.org/web/20220706091052/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStream.html)
*   [如何在 Java 中复制文件](/web/20220706091052/https://mkyong.com/java/how-to-copy-file-in-java/)
*   Apache common me
*   [在 Java 中把 InputStream 转换成 String](/web/20220706091052/https://mkyong.com/java/how-to-convert-inputstream-to-string-in-java/)
*   [在 Java 中将 InputStream 转换为 buffered reader](/web/20220706091052/https://mkyong.com/java/convert-inputstream-to-bufferedreader-in-java/)

<input type="hidden" id="mkyong-current-postId" value="2448">