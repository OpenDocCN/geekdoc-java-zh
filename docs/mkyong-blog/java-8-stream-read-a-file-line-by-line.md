# Java 8 流——逐行读取文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-stream-read-a-file-line-by-line/>

在 Java 8 中，你可以使用`Files.lines`作为`Stream`来读取文件。

c://lines.txt – A simple text file for testing

```java
 line1
line2
line3
line4
line5 
```

## 1.Java 8 读文件+流

TestReadFile.java

```java
 package com.mkyong.java8;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.stream.Stream;

public class TestReadFile {

	public static void main(String args[]) {

		String fileName = "c://lines.txt";

		//read file into stream, try-with-resources
		try (Stream<String> stream = Files.lines(Paths.get(fileName))) {

			stream.forEach(System.out::println);

		} catch (IOException e) {
			e.printStackTrace();
		}

	}

} 
```

输出

```java
 line1
line2
line3
line4
line5 
```

## 2.Java 8 读文件+流+额外

这个例子向你展示了如何使用`Stream`来过滤内容，将整个内容转换成大写字母，并作为一个`List`返回。

TestReadFile2.java

```java
 package com.mkyong.java8;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class TestReadFile2 {

	public static void main(String args[]) {

		String fileName = "c://lines.txt";
		List<String> list = new ArrayList<>();

		try (Stream<String> stream = Files.lines(Paths.get(fileName))) {

			//1\. filter line 3
			//2\. convert all content to upper case
			//3\. convert it into a List
			list = stream
					.filter(line -> !line.startsWith("line3"))
					.map(String::toUpperCase)
					.collect(Collectors.toList());

		} catch (IOException e) {
			e.printStackTrace();
		}

		list.forEach(System.out::println);

	}

} 
```

输出

```java
 LINE1
LINE2
LINE4
LINE5 
```

## 3.BufferedReader + Stream

从 1.8 开始增加了一个新方法`lines()`，它让`BufferedReader`以`Stream`的形式返回内容。

TestReadFile3.java

```java
 package com.mkyong.java8;

import java.io.BufferedReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class TestReadFile3{

	public static void main(String args[]) {

		String fileName = "c://lines.txt";
		List<String> list = new ArrayList<>();

		try (BufferedReader br = Files.newBufferedReader(Paths.get(fileName))) {

			//br returns as stream and convert it into a List
			list = br.lines().collect(Collectors.toList());

		} catch (IOException e) {
			e.printStackTrace();
		}

		list.forEach(System.out::println);

	}

} 
```

输出

```java
 line1
line2
line3
line4
line5 
```

## 4.经典缓冲阅读器和扫描仪

受够了 Java 8 和`Stream`，让我们重温一下经典的`BufferedReader` (JDK1.1)和`Scanner` (JDK1.5)的例子来逐行读取一个文件，它仍然在工作，只是开发者正在向`Stream`前进。

4.1 `BufferedReader` +资源尝试示例。

TestReadFile4.java

```java
 package com.mkyong.core;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class TestReadFile4{

	public static void main(String args[]) {

		String fileName = "c://lines.txt";

		try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {

			String line;
			while ((line = br.readLine()) != null) {
				System.out.println(line);
			}

		} catch (IOException e) {
			e.printStackTrace();
		}

	}

} 
```

4.2 `Scanner` +用资源试试看的例子。

TestReadFile5.java

```java
 package com.mkyong.core;

import java.io.File;
import java.io.IOException;
import java.util.Scanner;

public class TestReadFile5 {

	public static void main(String args[]) {

		String fileName = "c://lines.txt";

		try (Scanner scanner = new Scanner(new File(fileName))) {

			while (scanner.hasNext()){
				System.out.println(scanner.nextLine());
			}

		} catch (IOException e) {
			e.printStackTrace();
		}

	}

} 
```

## 参考

1.  [Java 8 File.lines()](http://web.archive.org/web/20210814180346/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#lines-java.nio.file.Path-)
2.  [Java 8 流](http://web.archive.org/web/20210814180346/https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
3.  [Java 缓冲器](http://web.archive.org/web/20210814180346/https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)

Tags : [java8](http://web.archive.org/web/20210814180346/https://mkyong.com/tag/java8/) [read file](http://web.archive.org/web/20210814180346/https://mkyong.com/tag/read-file/) [stream](http://web.archive.org/web/20210814180346/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="13887">