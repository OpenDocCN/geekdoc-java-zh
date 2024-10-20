# Java 8–如何将 IntStream 转换为 int 或 int[]

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-convert-intstream-to-int-or-int/>

从一个`IntStream`中获取一个原语`int`的几个 Java 8 例子。

## 1.IntStream -> int

Java8Example1.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.OptionalInt;
import java.util.stream.IntStream;

public class Java8Example1 {

    public static void main(String[] args) {

        int[] num = {1, 2, 3, 4, 5};

        //1\. int[] -> IntStream
        IntStream stream = Arrays.stream(num);

        // 2\. OptionalInt
        OptionalInt first = stream.findFirst();

        // 3\. getAsInt()
        int result = first.getAsInt();

        System.out.println(result);                                     // 1

        // one line
        System.out.println(Arrays.stream(num).findFirst().getAsInt());  // 1

    }

} 
```

输出

```java
 1
1 
```

Java8Example2.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.OptionalInt;
import java.util.stream.IntStream;

public class Java8Example2 {

    public static void main(String[] args) {

        int[] num = {1, 2, 3, 4, 5};

        //1\. int[] -> IntStream
        IntStream stream = Arrays.stream(num);

        // 2\. OptionalInt
        OptionalInt any = stream.filter(x -> x % 2 == 0).findAny();

        // 3\. getAsInt()
        int result = any.getAsInt();

        System.out.println(result); // 2 or 4

    }

} 
```

输出

```java
 2 or 4 
```

## 2.IntStream -> int[]或 Integer[]

Java8Example3.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.stream.IntStream;

public class Java8Example3 {

    public static void main(String[] args) {

        int[] num = {1, 2, 3, 4, 5};

        IntStream stream = Arrays.stream(num);

        // IntStream -> int[]
        int[] ints = stream.toArray();

        IntStream stream2 = Arrays.stream(num);

        // IntStream -> Integer[]
        Integer[] integers = stream2.boxed().toArray(Integer[]::new);

    }

} 
```

## 参考

*   [Java 8–如何将 IntStream 转换为整数数组](/web/20221127051005/https://mkyong.com/java8/java-8-how-to-convert-intstream-to-integer/)
*   [IntStream toArray()](http://web.archive.org/web/20221127051005/https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html#toArray--)

<input type="hidden" id="mkyong-current-postId" value="15505">