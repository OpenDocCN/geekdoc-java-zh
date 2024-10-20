# 如何在 Java 中将字符串转换成 InputStream

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-convert-string-to-inputstream-in-java/>

在 Java 中，我们可以使用`ByteArrayInputStream`将一个`String`转换成一个`InputStream`。

```java
 String str = "mkyong.com";
InputStream is = new ByteArrayInputStream(str.getBytes(StandardCharsets.UTF_8)); 
```

目录

*   [1\. ByteArrayInputStream](#bytearrayinputstream)
*   [2 .Apache common io–ioutils](#apache-commons-io-ioutils)
*   [3。下载源代码](#download-source-code)
*   [4。参考文献](#references)

## 1\. ByteArrayInputStream

这个例子使用`ByteArrayInputStream`将一个`String`转换成一个`InputStream`，并保存到一个文件中。

StringToInputStream.java

```java
 package com.mkyong.string;

import java.io.*;
import java.nio.charset.StandardCharsets;

public class ConvertStringToInputStream {

    public static final int DEFAULT_BUFFER_SIZE = 8192;

    public static void main(String[] args) throws IOException {

        String name = "mkyong.com";

        // String to InputStream
        InputStream is = new ByteArrayInputStream(name.getBytes(StandardCharsets.UTF_8));

        // save to a file
        save(is, "c:\\test\\file.txt");

    }

    // save the InputStream to a File
    private static void save(final InputStream is, final String fileName)
            throws IOException {

        // read bytes from InputStream and write it to FileOutputStream
        try (FileOutputStream outputStream =
                     new FileOutputStream(new File(fileName), false)) {
            int read;
            byte[] bytes = new byte[DEFAULT_BUFFER_SIZE];
            while ((read = is.read(bytes)) != -1) {
                outputStream.write(bytes, 0, read);
            }
        }

    }

} 
```

## 2 .Apache common io–ioutils

这个例子使用`commons-io`库、`IOUtils.toInputStream` API 将`String`转换为`InputStream`。

pom.xml

```java
 <dependency>
	<groupId>commons-io</groupId>
	<artifactId>commons-io</artifactId>
	<version>2.11.0</version>
</dependency> 
```

```java
 import org.apache.commons.io.IOUtils;

// String to InputStream
InputStream result = IOUtils.toInputStream(str, StandardCharsets.UTF_8); 
```

回顾一下`IOUtils.toInputStream`的源代码，它是用同一个`ByteArrayInputStream`把一个`String`转换成一个`InputStream`。

```java
 package org.apache.commons.io;

public class IOUtils {

	public static InputStream toInputStream(final String input, final Charset charset) {
		return new ByteArrayInputStream(input.getBytes(Charsets.toCharset(charset)));
	}

	//...
} 
```

**延伸阅读**
[Java 中把 InputStream 转换成 String](/web/20220619003336/https://mkyong.com/java/how-to-convert-inputstream-to-string-in-java/)

## 3。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003336/https://github.com/mkyong/core-java)

$ cd java-string

或者

$ cd java-io

## 4。参考文献

*   [ByteArrayOutputStream JavaDoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/ByteArrayOutputStream.html)
*   Apache common me
*   [如何在 Java 中将 InputStream 转换成文件](/web/20220619003336/https://mkyong.com/java/how-to-convert-inputstream-to-file-in-java/)

<input type="hidden" id="mkyong-current-postId" value="6844">