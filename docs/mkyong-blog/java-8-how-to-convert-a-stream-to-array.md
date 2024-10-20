# Java 8–将流转换为数组

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-convert-a-stream-to-array/>

在 Java 8 中，我们可以使用`.toArray()`将流转换成数组。

## 1.流->字符串[]

StreamString.java

```java
 package com.mkyong;

import java.util.Arrays;

public class StreamString {

    public static void main(String[] args) {

        String lines = "I Love Java 8 Stream!";

        // split by space, uppercase, and convert to Array
        String[] result = Arrays.stream(lines.split("\\s+"))
			.map(String::toUpperCase)
			.toArray(String[]::new);

        for (String s : result) {
            System.out.println(s);
        }

    }

} 
```

输出

```java
 I
LOVE
JAVA
8
STREAM! 
```

## 2.IntStream -> Integer[]或 int[]

2.1 流向`Integer[]`

StreamInt1.java

```java
 package com.mkyong;

import java.util.Arrays;

public class StreamInt1 {

    public static void main(String[] args) {

        int[] num = {1, 2, 3, 4, 5};

        Integer[] result = Arrays.stream(num)
			.map(x -> x * 2)
			.boxed()
			.toArray(Integer[]::new);

        System.out.println(Arrays.asList(result));

    }

} 
```

输出

```java
 [2, 4, 6, 8, 10] 
```

2.2 流至`int[]`

StreamInt2.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class StreamInt2 {

    public static void main(String[] args) {

        // IntStream -> int[]
        int[] stream1 = IntStream.rangeClosed(1, 5).toArray();
        System.out.println(Arrays.toString(stream1));

        // Stream<Integer> -> int[]
        Stream<Integer> stream2 = Stream.of(1, 2, 3, 4, 5);
        int[] result = stream2.mapToInt(x -> x).toArray();

        System.out.println(Arrays.toString(result));

    }

} 
```

输出

```java
 [1, 2, 3, 4, 5]
[1, 2, 3, 4, 5] 
```

## 参考

*   [Java–如何声明和初始化数组](/web/20210814144453/https://mkyong.com/java/java-how-to-declare-and-initialize-an-array/)
*   [Stream toArray()](http://web.archive.org/web/20210814144453/hhttps://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#toArray-java.util.function.IntFunction-)
*   [Java 8–将流转换为列表](/web/20210814144453/https://mkyong.com/java8/java-8-convert-a-stream-to-list/)

Tags : [array](http://web.archive.org/web/20210814144453/https://mkyong.com/tag/array/) [java 8](http://web.archive.org/web/20210814144453/https://mkyong.com/tag/java-8/) [stream](http://web.archive.org/web/20210814144453/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="14974">