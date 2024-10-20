# Java 8–将流转换为列表

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-a-stream-to-list/>

一个 Java 8 的例子，展示了如何通过`Collectors.toList`将`Stream`转换成`List`

Java8Example1.java

```java
 package com.mkyong.java8;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Java8Example1 {

    public static void main(String[] args) {

        Stream<String> language = Stream.of("java", "python", "node");

        //Convert a Stream to List
        List<String> result = language.collect(Collectors.toList());

        result.forEach(System.out::println);

    }
} 
```

输出

```java
 java
python
node 
```

还有一个例子，过滤一个数字 3 并将其转换成一个列表。

Java8Example2.java

```java
 package com.mkyong.java8;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Java8Example2 {

    public static void main(String[] args) {

        Stream<Integer> number = Stream.of(1, 2, 3, 4, 5);

        List<Integer> result2 = number.filter(x -> x != 3).collect(Collectors.toList());

        result2.forEach(x -> System.out.println(x));

    }
} 
```

输出

```java
 1
2
4
5 
```

## 参考

*   [Java 8 收集器 JavaDoc](http://web.archive.org/web/20210815163519/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
*   [Java 8 流过滤器示例](/web/20210815163519/https://mkyong.com/java8/java-8-streams-filter-examples/)
*   [Java 8–如何将流转换成数组](/web/20210815163519/https://mkyong.com/java8/java-8-how-to-convert-a-stream-to-array/)

Tags : [convert](http://web.archive.org/web/20210815163519/https://mkyong.com/tag/convert/) [filter](http://web.archive.org/web/20210815163519/https://mkyong.com/tag/filter/) [java 8](http://web.archive.org/web/20210815163519/https://mkyong.com/tag/java-8/) [list](http://web.archive.org/web/20210815163519/https://mkyong.com/tag/list/) [stream](http://web.archive.org/web/20210815163519/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="14035">