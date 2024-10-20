# 如何在 Java 中读取文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-read-file-from-java-bufferedreader-example/>

在本文中，我们将向您展示如何使用`java.io.BufferedReader`从文件中读取内容

**Note**
Read this [different ways read a file](http://web.archive.org/web/20220619003336/https://www.mkyong.com/java/java-how-to-read-a-file/)

## 1.Files.newBufferedReader (Java 8)

在 Java 8 中，有一个新的方法`Files.newBufferedReader(Paths.get("file"))`来返回一个`BufferedReader`

filename.txt

```java
 A
B
C
D
E 
```

FileExample1.java

```java
 package com.mkyong;

import java.io.BufferedReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class FileExample1 {

    public static void main(String[] args) {

        StringBuilder sb = new StringBuilder();

        try (BufferedReader br = Files.newBufferedReader(Paths.get("filename.txt"))) {

            // read line by line
            String line;
            while ((line = br.readLine()) != null) {
                sb.append(line).append("\n");
            }

        } catch (IOException e) {
            System.err.format("IOException: %s%n", e);
        }

        System.out.println(sb);

    }

} 
```

输出

```java
 A
B
C
D
E 
```

## 2.缓冲阅读器

2.1 经典的`BufferedReader`与 JDK 1.7 `try-with-resources`自动关闭资源。

FileExample2.java

```java
 package com.mkyong;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class FileExample2 {

    public static void main(String[] args) {

        try (FileReader reader = new FileReader("filename.txt");
             BufferedReader br = new BufferedReader(reader)) {

            // read line by line
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }

        } catch (IOException e) {
            System.err.format("IOException: %s%n", e);
        }
    }

} 
```

2.2 在过去，我们必须手动关闭所有内容。

FileExample3.java

```java
 package com.mkyong.calculator;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class FileExample3 {

    public static void main(String[] args) {

        BufferedReader br = null;
        FileReader fr = null;

        try {

            fr = new FileReader("filename.txt");
            br = new BufferedReader(fr);

            // read line by line
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }

        } catch (IOException e) {
            System.err.format("IOException: %s%n", e);
        } finally {
            try {
                if (br != null)
                    br.close();

                if (fr != null)
                    fr.close();
            } catch (IOException ex) {
                System.err.format("IOException: %s%n", ex);
            }
        }

    }

} 
```

## 参考

*   [文件 JavaDocs](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)
*   [BufferedReader JavaDocs](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)
*   [Java–如何读取文件](http://web.archive.org/web/20220619003336/https://www.mkyong.com/java/java-how-to-read-a-file/)
*   [try-with-resources 语句 javadoc](http://web.archive.org/web/20220619003336/https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

<input type="hidden" id="mkyong-current-postId" value="452">