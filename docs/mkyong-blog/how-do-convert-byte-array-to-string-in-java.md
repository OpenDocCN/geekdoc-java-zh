# 如何在 Java 中将 byte[]数组转换成字符串

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-do-convert-byte-array-to-string-in-java/>

在 Java 中，我们可以使用`new String(bytes, StandardCharsets.UTF_8)`将一个`byte[]`转换成一个`String`。

```java
 // string to byte[]
  byte[] bytes = "hello".getBytes(StandardCharsets.UTF_8);

  // byte[] to string
  String s = new String(bytes, StandardCharsets.UTF_8); 
```

目录

*   [1。文本和二进制数据中的字节[]](#byte-in-text-and-binary-data)
*   [2。将字节[]转换为字符串(文本数据)](#convert-byte-to-string-text-data)
*   [3。将字节[]转换为字符串(二进制数据)](#convert-byte-to-string-binary-data)
*   [4。下载源代码](#download-source-code)
*   [5。参考文献](#references)

## 1。文本和二进制数据中的字节[]

对于文本或字符数据，我们使用`new String(bytes, StandardCharsets.UTF_8)`将`byte[]`直接转换为`String`。然而，对于`byte[]`保存图像或其他非文本数据等二进制数据的情况，最佳实践是将`byte[]`转换为 [Base64](http://web.archive.org/web/20220831131853/https://en.wikipedia.org/wiki/Base64) 编码的字符串。

```java
 // convert file to byte[]
  byte[] bytes = Files.readAllBytes(Paths.get("/path/image.png"));

  // Java 8 - Base64 class, finally.

  // encode, convert byte[] to base64 encoded string
  String s = Base64.getEncoder().encodeToString(bytes);

  System.out.println(s);

  // decode, convert base64 encoded string back to byte[]
  byte[] decode = Base64.getDecoder().decode(s);

  // This Base64 encode decode string is still widely use in
  // 1\. email attachment
  // 2\. embed image files inside HTML or CSS 
```

**注**

*   对于`text data byte[]`，试试`new String(bytes, StandardCharsets.UTF_8)`。
*   对于`binary data byte[]`，尝试 Base64 编码。

## 2。将字节[]转换为字符串(文本数据)

以下示例将一个`string`转换为一个字节数组或`byte[]`，反之亦然。

**警告**
常见的错误是试图使用`bytes.toString()`从字节中获取字符串；`bytes.toString()`只返回对象在内存中的地址，不会将`byte[]`转换成`string`！将`byte[]`转换成`string`的正确方法是`new String(bytes, StandardCharsets.UTF_8)`。

ConvertBytesToString2.java

```java
 package com.mkyong.string;

import java.nio.charset.StandardCharsets;

public class ConvertBytesToString2 {

  public static void main(String[] args) {

      String str = "This is raw text!";

      // string to byte[]
      byte[] bytes = str.getBytes(StandardCharsets.UTF_8);

      System.out.println("Text : " + str);
      System.out.println("Text [Byte Format] : " + bytes);

      // no, don't do this, it returns the address of the object in memory
      System.out.println("Text [Byte Format] toString() : " + bytes.toString());

      // convert byte[] to string
      String s = new String(bytes, StandardCharsets.UTF_8);
      System.out.println("Output : " + s);

      // old code, UnsupportedEncodingException
      // String s1 = new String(bytes, "UTF_8");

  }

} 
```

输出

Terminal

```java
 Text : This is raw text!
Text [Byte Format] : [B@372f7a8d
Text [Byte Format] toString() : [B@372f7a8d
Output : This is raw text! 
```

## 3。将字节[]转换为字符串(二进制数据)

下面的示例将图像`phone.png`转换为`byte[]`，并使用 Java 8 `Base64`类将`byte[]`转换为 Base64 编码的字符串。

稍后，我们将 Base64 编码的字符串转换回原始的`byte[]`，并将其保存到另一个名为`phone2.png`的图像中。

ConvertBytesToStringBase64.java

```java
 package com.mkyong.string;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Base64;

public class ConvertBytesToStringBase64 {

    public static void main(String[] args) {

        String filepath = "/Users/mkyong/phone.png";
        Path path = Paths.get(filepath);

        if (Files.notExists(path)) {
            throw new IllegalArgumentException("File is not exists!");
        }

        try {

            // convert the file's content to byte[]
            byte[] bytes = Files.readAllBytes(path);

            // encode, byte[] to Base64 encoded string
            String s = Base64.getEncoder().encodeToString(bytes);
            System.out.println(s);

            // decode, Base64 encoded string to byte[]
            byte[] decode = Base64.getDecoder().decode(s);

            // save into another image file.
            Files.write(Paths.get("/Users/mkyong/phone2.png"), decode);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 bh5aLyZALN4othXL2mByHo1aZA5ts5k/uw/sc7DBngGY......

  # if everything ok, it save the byte[] into a new image phone2.png 
```

## 4。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220831131853/https://github.com/mkyong/core-java)

$ cd java-string

## 5。参考文献

*   [Java 中如何将字符串转换成字节[]](/web/20220831131853/https://mkyong.com/java/how-do-convert-string-to-byte-in-java/)
*   [Java 11–String JavaDoc](http://web.archive.org/web/20220831131853/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html)
*   [维基百科–Base64](http://web.archive.org/web/20220831131853/https://en.wikipedia.org/wiki/Base64)

<input type="hidden" id="mkyong-current-postId" value="1053">