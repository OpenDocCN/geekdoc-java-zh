# Java–将文件转换为十六进制

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-convert-file-to-hex-in-java/>

这篇文章展示了如何将一个文件转换成一个[十六进制(hex)](http://web.archive.org/web/20220619003351/https://en.wikipedia.org/wiki/Hexadecimal) 的代表格式。

例如，下面是一个文本文件。

/path/to/text.txt

```java
 ABCDEFG
12345678
!@#$%^&*()
Testing only 
```

我们将把上面的文件转换成下面的十六进制格式。

```java
 41 42 43 44 45 46 47 0D 0A 31 32 33 34 35 36                 | ABCDEFG..123456
37 38 0D 0A 21 40 23 24 25 5E 26 2A 28 29 0D                 | 78..!@#$%^&*().
0A 54 65 73 74 69 6E 67 20 6F 6E 6C 79                       | .Testing only 
```

## 1.Java 将文件转换为十六进制

想法是将文件读入一个`InputStream`，并使用 [String.format(%X)](http://web.archive.org/web/20220619003351/https://mkyong.com/java/java-string-format-examples/) 将每个字节转换成一个十六进制代码。

FileToHex.java

```java
 package com.mkyong.io.howto;

import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class FileToHex {

    private static final String NEW_LINE = System.lineSeparator();
    private static final String UNKNOWN_CHARACTER = ".";

    public static void main(String[] args) throws IOException {

        String file = "/path/to/text.txt";

        String s = convertFileToHex(Paths.get(file));
        System.out.println(s);
    }

    public static String convertFileToHex(Path path) throws IOException {

        if (Files.notExists(path)) {
            throw new IllegalArgumentException("File not found! " + path);
        }

        StringBuilder result = new StringBuilder();
        StringBuilder hex = new StringBuilder();
        StringBuilder input = new StringBuilder();

        int count = 0;
        int value;

        // path to inputstream....
        try (InputStream inputStream = Files.newInputStream(path)) {

            while ((value = inputStream.read()) != -1) {

                hex.append(String.format("%02X ", value));

                //If the character is unable to convert, just prints a dot "."
                if (!Character.isISOControl(value)) {
                    input.append((char) value);
                } else {
                    input.append(UNKNOWN_CHARACTER);
                }

                // After 15 bytes, reset everything for formatting purpose
                if (count == 14) {
                    result.append(String.format("%-60s | %s%n", hex, input));
                    hex.setLength(0);
                    input.setLength(0);
                    count = 0;
                } else {
                    count++;
                }

            }

            // if the count>0, meaning there is remaining content
            if (count > 0) {
                result.append(String.format("%-60s | %s%n", hex, input));
            }

        }

        return result.toString();
    }

} 
```

## 2.将图像文件转换为十六进制

使用图像文件运行上述程序。

```java
 String file = "/path/to/hello.png"; 
```

输出

```java
 89 50 4E 47 0D 0A 1A 0A 00 00 00 0D 49 48 44                 | .PNG........IHD
52 00 00 03 11 00 00 01 1E 08 06 00 00 00 F5                 | R.............õ
AE 98 9A 00 00 00 09 70 48 59 73 00 00 12 74                 | ®......pHYs...t
00 00 12 74 01 DE 66 1F 78 00 00 00 07 74 49                 | ...t.Þf.x....tI

//...

20 08 42 4B 88 13 21 08 82 20 08 82 20 08 42                 |  .BK..!.. .. .B
4B FC 7F 0B 00 ED 81 F6 3A 58 EC 00 00 00 00                 | Kü...í.ö:Xì....
49 45 4E 44 AE 42 60 82                                      | IEND®B`. 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220619003351/https://github.com/mkyong/core-java)

$ cd java-io/howto

## 参考

*   [WIkipedia – Hexadecimal](http://web.archive.org/web/20220619003351/https://en.wikipedia.org/wiki/Hexadecimal)
*   [String.format(%X)和十六进制代码](http://web.archive.org/web/20220619003351/https://mkyong.com/java/java-string-format-examples/)
*   [文件 JavaDoc](http://web.archive.org/web/20220619003351/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html)
*   [路径 JavaDoc](http://web.archive.org/web/20220619003351/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Path.html)

<input type="hidden" id="mkyong-current-postId" value="6819">