# 如何在 Java 中向文件追加文本

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-append-content-to-file-in-java/>

本文展示了如何使用以下 Java APIs 将文本追加到文件末尾。

1.  `Files.write`–向文件追加一行，Java 7。
2.  `Files.write`–在一个文件中追加多行，Java 7，Java 8。
3.  `Files.writeString`–Java 11。
4.  `FileWriter`
5.  `FileOutputStream`
6.  `FileUtils`——Apache common io。

在 Java 中，对于像`Files.write`这样的 NIO APIs，我们可以使用 [StandardOpenOption。追加](http://web.archive.org/web/20221023054404/https://docs.oracle.com/javase/8/docs/api/java/nio/file/StandardOpenOption.html)启用追加模式。例如:

```java
 // append a string to the end of the file
	private static void appendToFile(Path path, String content)
		  throws IOException {

			Files.write(path,
							content.getBytes(StandardCharsets.UTF_8),
							StandardOpenOption.CREATE,
							StandardOpenOption.APPEND);

	} 
```

对于像`FileWriter`或`FileOutputStream`这样的经典 IO APIs，我们可以向构造函数的第二个参数传递一个`true`来启用追加模式。例如:

```java
 // append to the file
	try (FileWriter fw = new FileWriter(file, true);
       BufferedWriter bw = new BufferedWriter(fw)) {
      bw.write(content);
      bw.newLine();
  }

	// append to the file
	try (FileOutputStream fos = new FileOutputStream(file, true)) {
      fos.write(content.getBytes(StandardCharsets.UTF_8));
  } 
```

## 1.向文件追加一行——files . write

如果文件不存在，API 抛出`NoSuchFileException`

```java
 Files.write(path, content.getBytes(StandardCharsets.UTF_8),
	                StandardOpenOption.APPEND); 
```

更好的解决方案总是结合了`StandardOpenOption.CREATE`和`StandardOpenOption.APPEND`。如果文件不存在，API 将创建文本并写入文件；如果文件存在，将文本追加到文件的末尾。

```java
 Files.write(path, content.getBytes(StandardCharsets.UTF_8),
	                StandardOpenOption.CREATE,
	                StandardOpenOption.APPEND); 
```

1.1 下面的例子展示了如何在一个文件的末尾添加一行。

FileAppend1.java

```java
 package com.mkyong.io.file;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class FileAppend1 {

    private static final String NEW_LINE = System.lineSeparator();

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("/home/mkyong/test/abc.txt");
        appendToFile(path, "hello world" + NEW_LINE);

    }

    // Java 7
    private static void appendToFile(Path path, String content)
				throws IOException {

        // if file not exists throws java.nio.file.NoSuchFileException
        /* Files.write(path, content.getBytes(StandardCharsets.UTF_8),
						StandardOpenOption.APPEND);*/

        // if file not exists, create and write to it
				// otherwise append to the end of the file
        Files.write(path, content.getBytes(StandardCharsets.UTF_8),
                StandardOpenOption.CREATE,
                StandardOpenOption.APPEND);

    }

} 
```

输出

第一次运行。

/home/mkyong/test/abc.txt

```java
 hello world 
```

运行第二次。

/home/mkyong/test/abc.txt

```java
 hello world
hello world 
```

## 2.将多行追加到文件–files . write

`Files.write`也支持多行的`Iterable`接口，我们可以在一个文件中添加一个`List`。

```java
 // append lines of text
    private static void appendToFileJava8(Path path, List<String> list)
			throws IOException {

        // Java 7?
        /*Files.write(path, list, StandardCharsets.UTF_8,
                StandardOpenOption.CREATE,
                StandardOpenOption.APPEND);*/

        // Java 8, default utf_8
        Files.write(path, list,
                StandardOpenOption.CREATE,
                StandardOpenOption.APPEND);

    } 
```

## 3.Java 11–追加模式下的 Files.writeString。

在 Java 7 中，我们需要将一个`String`转换成一个`byte[]`，并将其写入或追加到一个文件中。

```java
 String content = "...";

	Files.write(path, content.getBytes(StandardCharsets.UTF_8),
					StandardOpenOption.CREATE,
					StandardOpenOption.APPEND); 
```

在 Java 11 中，我们可以使用新的`Files.writeString` API 将字符串直接写入或追加到文件中。追加模式的工作方式相同。

```java
 // Java 11, writeString, append mode
  private static void appendToFileJava11(Path path, String content)
			throws IOException {

      // utf_8
      /*Files.writeString(path, content, StandardCharsets.UTF_8,
              StandardOpenOption.CREATE,
              StandardOpenOption.APPEND);*/

      // default StandardCharsets.UTF_8
      Files.writeString(path, content,
              StandardOpenOption.CREATE,
              StandardOpenOption.APPEND);
  } 
```

## 4.文件写入器

对于像`FileWriter`这样的遗留 IO APIs，构造函数的第二个参数表示追加模式。

```java
 // append
	// if file not exists, create and write
	// if the file exists, append to the end of the file
	try (FileWriter fw = new FileWriter(file, true);
			 BufferedWriter bw = new BufferedWriter(fw)) {

			bw.write(content);
			bw.newLine();   // add new line, System.lineSeparator()

	} 
```

4.1 下面的例子显示了如何使用`FileWriter`在文件末尾添加一行。

FileAppend4.java

```java
 package com.mkyong.io.file;

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileAppend4 {

	public static void main(String[] args) throws IOException {

			File file = new File("/home/mkyong/test/abc.txt");
			appendToFileFileWriter(file, "hello world");
			System.out.println("Done");

	}

	private static void appendToFileFileWriter(File file, String content)
			throws IOException {

			// default - create and write
			// if file not exists, create and write
			// if file exists, truncate and write
			/*try (FileWriter fw = new FileWriter(file);
					 BufferedWriter bw = new BufferedWriter(fw)) {
					bw.write(content);
					bw.newLine();
			}*/

			// append mode
			// if file not exists, create and write
			// if file exists, append to the end of the file
			try (FileWriter fw = new FileWriter(file, true);
					 BufferedWriter bw = new BufferedWriter(fw)) {

					bw.write(content);
					bw.newLine();   // add new line, System.lineSeparator()

			}

	}

} 
```

4.2 以下示例将一个`List`或多行追加到文件末尾。

```java
 private static void appendToFileFileWriter(
			File file, List<String> content) throws IOException {

      try (FileWriter fw = new FileWriter(file, true);
           BufferedWriter bw = new BufferedWriter(fw)) {

          for (String s : content) {
              bw.write(s);
              bw.newLine();
          }
      }

  } 
```

## 5.文件输出流

`FileOutputStream`的附加模式与`FileWriter`的工作方式相同。

```java
 private static void appendToFileFileOutputStream(File file, String content)
			throws IOException {

      // append mode
      try (FileOutputStream fos = new FileOutputStream(file, true)) {
          fos.write(content.getBytes(StandardCharsets.UTF_8));
      }

  } 
```

## 6\. FileUtils

下面的例子使用流行的 Apache commons-io `FileUtils.writeStringToFile`在文件末尾添加一个字符串。

pom.xml

```java
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.7</version>
  </dependency> 
```

```java
 import org.apache.commons.io.FileUtils;

    private static void appendToFileFileUtils(File file, String content)
			  throws IOException {

				// append mode
        FileUtils.writeStringToFile(
                file, content, StandardCharsets.UTF_8, true);

    } 
```

审核`FileUtils.writeStringToFile`签名；最后一个或第四个参数表示追加模式。

```java
 public static void writeStringToFile(final File file, final String data,
				final Charset charset,final boolean append)
				throws IOException {

      try (OutputStream out = openOutputStream(file, append)) {
          IOUtils.write(data, out, charset);
      }

  } 
```

## 7.在过去。

在 Java 7 之前，我们可以使用`FileWriter`将文本添加到文件中，并手动关闭资源。

ClassicBufferedWriterExample.java

```java
 package com.mkyong;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class ClassicBufferedWriterExample {

    public static void main(String[] args) {

        BufferedWriter bw = null;
        FileWriter fw = null;

        try {

            String content = "Hello";

            fw = new FileWriter("app.log", true);
            bw = new BufferedWriter(fw);
            bw.write(content);

        } catch (IOException e) {
            System.err.format("IOException: %s%n", e);
        } finally {
            try {
                if (bw != null)
                    bw.close();

                if (fw != null)
                    fw.close();
            } catch (IOException ex) {
                System.err.format("IOException: %s%n", ex);
            }
        }
    }
} 
```

*附:以上代码只是为了好玩和遗留目的，始终坚持 try-with-resources 关闭资源。*

## 参考

*   [Files.write Java Docs](http://web.archive.org/web/20221023054404/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#write(java.nio.file.Path,byte%5B%5D,java.nio.file.OpenOption...))
*   [FileWriter JavaDocs](http://web.archive.org/web/20221023054404/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileWriter.html)
*   [FileOutputStream JavaDoc](http://web.archive.org/web/20221023054404/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileOutputStream.html)
*   [StandardOpenOption JavaDocs](http://web.archive.org/web/20221023054404/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/StandardOpenOption.html)
*   Apache common me
*   [Java–如何创建和写入文件](/web/20221023054404/https://mkyong.com/java/java-how-to-create-and-write-to-a-file/)

<input type="hidden" id="mkyong-current-postId" value="5447">