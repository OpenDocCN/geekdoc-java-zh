# Java–将文件转换为字符串

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-convert-file-to-string/>

在 Java 中，我们有很多方法将文件转换成字符串。

用于以后测试的文本文件。

c:\\projects\\app.log

```java
 A
B
C
D
E 
```

## 1.Java 11–files . read string

在`java.nio.file.Files`中增加了一个新的方法`Files.readString`，它使得从文件中读取字符串变得更加容易。

FileToString1.java

```java
 package com.mkyong;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class FileToString1 {

    public static void main(String[] args) {

        String path = "c:\\projects\\app.log";

        try {

            // default StandardCharsets.UTF_8
            String content = Files.readString(Paths.get(path));
            System.out.println(content);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

Terminal

```java
 A
B
C
D
E 
```

## 2.Java 8–文件.行

将一个文件转换成一个`Stream`并连接它。

FileToString2.java

```java
 package com.mkyong;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FileToString2 {

    public static void main(String[] args) {

        String path = "c:\\projects\\app.log";

        try (Stream<String> lines = Files.lines(Paths.get(path))) {

            // Formatting like \r\n will be lost
            // String content = lines.collect(Collectors.joining());

            // UNIX \n, WIndows \r\n
            String content = lines.collect(Collectors.joining(System.lineSeparator()));
            System.out.println(content);

			// File to List
            //List<String> list = lines.collect(Collectors.toList());

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

Terminal

```java
 A
B
C
D
E 
```

## 3.Java 7–files . read all lines

将文件转换成`List`

FileExample3.java

```java
 package com.mkyong;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class FileToString3 {

    public static void main(String[] args) {

        String path = "c:\\projects\\app.log";

        try {

            // default StandardCharsets.UTF_8
            // Java 8
            // List<String> content = Files.readAllLines(Paths.get(path));

			// Java 7
            List<String> content = Files.readAllLines(Paths.get(path), StandardCharsets.UTF_8);

            System.out.println(content);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 [A, B, C, D, E] 
```

## 4.Apache commons me(Apache 公用程式)

pom.xml

```java
 <dependency>
		<groupId>commons-io</groupId>
		<artifactId>commons-io</artifactId>
		<version>2.6</version>
	</dependency> 
```

FileToString4.java

```java
 package com.mkyong;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class FileToString4 {

    public static void main(String[] args) {

        String path = "c:\\projects\\app.log";

        try {
            String content = FileUtils.readFileToString(new File(path), StandardCharsets.UTF_8);
            System.out.println(content);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 A
B
C
D
E 
```

## 参考

*   [文件 JavaDocs](http://web.archive.org/web/20220619003338/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)
*   [BufferedReader JavaDocs](http://web.archive.org/web/20220619003338/https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)
*   [如何在 Java 中读取文件–buffered reader](http://web.archive.org/web/20220619003338/https://www.mkyong.com/java/how-to-read-file-from-java-bufferedreader-example/)
*   [Java 8 流–逐行读取文件](http://web.archive.org/web/20220619003338/https://www.mkyong.com/java8/java-8-stream-read-a-file-line-by-line/)

<input type="hidden" id="mkyong-current-postId" value="15091">