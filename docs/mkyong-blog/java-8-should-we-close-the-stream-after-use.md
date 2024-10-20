# Java 8——我们应该在使用后关闭流吗？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-8-should-we-close-the-stream-after-use/>

只有来源为 IO 通道(如`Files.lines(Path, Charset)`)的流才需要关闭。

阅读这个[流 JavaDocs](http://web.archive.org/web/20210814151230/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)

> 流有一个 BaseStream.close()方法并实现 AutoCloseable，但是几乎所有的流实例在使用后都不需要关闭。通常，只有来源为 IO 通道的流(如 Files.lines(Path，Charset)返回的流)才需要关闭。大多数流由集合、数组或生成函数支持，不需要特殊的资源管理。(如果流确实需要关闭，可以在 try-with-resources 语句中将其声明为资源。)

1.对于像这样的普通流，流实例在使用后不需要关闭。

Java8StreamExample.java

```java
 package com.mkyong;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Java8StreamExample {

    public static void main(String[] args) {

        Stream<String> stream = Stream.of("A", "B", "C");

        List<String> filter = stream.filter(x -> !x.equalsIgnoreCase("B"))
			.collect(Collectors.toList());

        // no need close the stream.
        //stream.close();

        System.out.println(filter); // [A, C]

    }

} 
```

2.对于来源为 IO 通道的流，用`try-with-resources`将其关闭

Java8StreamIO.java

```java
 package com.mkyong;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Java8StreamIO {

    public static void main(String[] args) {

        String path = "c:\\projects\\app.log";

		// auto close
        try (Stream<String> lines = Files.lines(Paths.get(path))) {

            String content = lines.collect(Collectors.joining(System.lineSeparator()));

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

## 参考

*   [流 JavaDocs](http://web.archive.org/web/20210814151230/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
*   [Java 8 流–逐行读取文件](http://web.archive.org/web/20210814151230/https://www.mkyong.com/java8/java-8-stream-read-a-file-line-by-line/)

Tags : [java 8](http://web.archive.org/web/20210814151230/https://mkyong.com/tag/java-8/) [stream](http://web.archive.org/web/20210814151230/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="15092">