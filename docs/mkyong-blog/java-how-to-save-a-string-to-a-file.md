# Java——如何将字符串保存到文件中

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-save-a-string-to-a-file/>

在 Java 中，有许多方法可以将字符串写入文件。

## 1.Java 11–files . writestring

最后，在`java.nio`中增加了一个新方法，可以轻松地将字符串保存到`File`中。

StringToFileJava11.java

```java
 package com.mkyong;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class StringToFileJava11 {

    public static void main(String[] args) {

        String content = "Hello World \r\nJava!\r\n";
        String path = "c:\\projects\\app.log";

        try {

            // Java 11 , default StandardCharsets.UTF_8
            Files.writeString(Paths.get(path), content);

            // encoding
            // Files.writeString(Paths.get(path), content, StandardCharsets.US_ASCII);

            // extra options
            // Files.writeString(Paths.get(path), content, 
			//		StandardOpenOption.CREATE, StandardOpenOption.APPEND);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

c:\\projects\\app.log

```java
 Hello World 
Java! 
```

## 2.Java 7–文件.写入

StringToFileJava7.java

```java
 package com.mkyong;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class StringToFileJava7 {

    public static void main(String[] args) {

        String content = "Hello World \r\nJava!\r\n";
        String path = "c:\\projects\\app.log";

        try {

            // Java 7
            Files.write(Paths.get(path), content.getBytes());

            // encoding
            // Files.write(Paths.get(path), content.getBytes(StandardCharsets.UTF_8));

            // extra options
            // Files.write(Paths.get(path), content.getBytes(), 
			//		StandardOpenOption.CREATE, StandardOpenOption.APPEND);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

c:\\projects\\app.log

```java
 Hello World 
Java! 
```

## 3.Apache commons me(Apache 公用程式)

pom.xml

```java
 <dependency>
		<groupId>commons-io</groupId>
		<artifactId>commons-io</artifactId>
		<version>2.6</version>
	</dependency> 
```

CommonsIOExample.java

```java
 package com.mkyong;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class CommonsIOExample {

    public static void main(String[] args) {

        String content = "Hello World \r\nJava!\r\n";
        String path = "c:\\projects\\app2.log";

        try {
            FileUtils.writeStringToFile(new File(path), content, StandardCharsets.UTF_8);

            // append
            // FileUtils.writeStringToFile(new File(path), content, StandardCharsets.UTF_8, true);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

} 
```

## 4.缓冲写入器

4.1 在过去:

BufferedWriterExample.java

```java
 package com.mkyong;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterExample {

    public static void main(String[] args) {

        String content = "Hello World \r\nJava!\r\n";
        String path = "c:\\projects\\app.log";

        try (FileWriter writer = new FileWriter(path);
              BufferedWriter bw = new BufferedWriter(writer)) {

            bw.write(content);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

4.2 或者像这样，手动关闭所有资源🙂

BufferedWriterExampleBeforeJava7.java

```java
 package com.mkyong;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterExampleBeforeJava7 {

    public static void main(String[] args) {

        String content = "Hello World \r\nJava!\r\n";
        String path = "c:\\projects\\app.log";

        BufferedWriter bw = null;
        FileWriter fw = null;

        try {

            fw = new FileWriter(path);
            bw = new BufferedWriter(fw);

            bw.write(content);

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (bw != null)
                    bw.close();

                if (fw != null)
                    fw.close();

            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }
    }

} 
```

## 参考

*   [文件 Java 文档](http://web.archive.org/web/20220629160035/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)
*   [Java 教程——阅读、编写和创建文件](http://web.archive.org/web/20220629160035/https://docs.oracle.com/javase/tutorial/essential/io/file.html)
*   [BufferedWriter JavaDocs](http://web.archive.org/web/20220629160035/https://docs.oracle.com/javase/8/docs/api/java/io/BufferedWriter.html)
*   [Java——如何将文本添加到文件中](http://web.archive.org/web/20220629160035/https://www.mkyong.com/java/java-how-to-append-text-to-a-file/)
*   [Java–如何创建和写入文件](http://web.archive.org/web/20220629160035/https://www.mkyong.com/java/java-how-to-create-and-write-to-a-file/)

<input type="hidden" id="mkyong-current-postId" value="15090">