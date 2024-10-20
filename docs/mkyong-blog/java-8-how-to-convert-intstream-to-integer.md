# Java 8–如何将 IntStream 转换为 Integer[]

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-convert-intstream-to-integer/>

关键是`boxed()`把`IntStream`转换成`Stream<Integer>`，然后只转换成一个数组。

StreamExample.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class StreamExample {

    public static void main(String[] args) {

        //int[] -> IntStream -> Stream<Integer> -> Integer[]
        int[] num = {3, 4, 5};

        //1\. int[] -> IntStream
        IntStream stream = Arrays.stream(num);

        //2\. IntStream -> Stream<Integer>
        Stream<Integer> boxed = stream.boxed();

        //3\. Stream<Integer> -> Integer[]
        Integer[] result = boxed.toArray(Integer[]::new);

        System.out.println(Arrays.toString(result));

        // one line
        Integer[] oneLineResult = Arrays.stream(num).boxed().toArray(Integer[]::new);
        System.out.println(Arrays.toString(oneLineResult));
    }

} 
```

输出

```java
 [3, 4, 5]
[3, 4, 5] 
```

## 参考

*   [Java–如何声明和初始化数组](/web/20210819034029/https://mkyong.com/java/java-how-to-declare-and-initialize-an-array/)
*   [Stream toArray()](http://web.archive.org/web/20210819034029/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#toArray-java.util.function.IntFunction-)
*   [流盒装()](http://web.archive.org/web/20210819034029/https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#boxed--)

Tags : [array](http://web.archive.org/web/20210819034029/https://mkyong.com/tag/array/) [intstream](http://web.archive.org/web/20210819034029/https://mkyong.com/tag/intstream/) [java 8](http://web.archive.org/web/20210819034029/https://mkyong.com/tag/java-8/) [stream](http://web.archive.org/web/20210819034029/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="14973">