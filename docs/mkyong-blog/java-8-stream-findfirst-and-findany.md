# Java 8 流 findFirst()和 findAny()

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-stream-findfirst-and-findany/>

在 Java 8 流中，`findFirst()`返回流中的第一个元素，而`findAny()`返回流中的任何元素。

## 1.findFirst()

1.1 从整数流中找到第一个元素。

Java8FindFirstExample1.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Java8FindFirstExample1 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 2, 1);

        Optional<Integer> first = list.stream().findFirst();
        if (first.isPresent()) {
            Integer result = first.get();
            System.out.println(result);       // 1
        } else {
            System.out.println("no value?");
        }

        Optional<Integer> first2 = list
                .stream()
                .filter(x -> x > 1).findFirst();

        if (first2.isPresent()) {
            System.out.println(first2.get()); // 2
        } else {
            System.out.println("no value?");
        }
    }

} 
```

输出

```java
 1
2 
```

1.2 从字符串流中找到不等于“node”的第一个元素。

Java8FindFirstExample2.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Java8FindFirstExample2 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("node", "java", "python", "ruby");

        Optional<String> result = list.stream()
                .filter(x -> !x.equalsIgnoreCase("node"))
                .findFirst();

        if (result.isPresent()) {
            System.out.println(result.get()); // java
        } else {
            System.out.println("no value?");
        }

    }

} 
```

输出

```java
 java 
```

## 2\. findAny()

2.1 从整数流中找出任意元素。如果我们运行下面的程序，大多数情况下，结果是 2，看起来`findAny()`总是返回第一个元素？但是，**不能保证**能做到这一点，`findAny()`可以从流中返回任何元素。

Java8FindAnyExample1.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Java8FindAnyExample1 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        Optional<Integer> any = list.stream().filter(x -> x > 1).findAny();
        if (any.isPresent()) {
            Integer result = any.get();
            System.out.println(result);
        }

    }

} 
```

输出

```java
 2 // no guaranteed 
```

## 参考

*   [Java 8 流过滤器示例](/web/20221205123008/https://mkyong.com/java8/java-8-streams-filter-examples/)
*   [Java 8 Stream JavaDoc](http://web.archive.org/web/20221205123008/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
*   [stream . findfirst()JavaDoc](http://web.archive.org/web/20221205123008/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#findFirst--)
*   [Stream.findAny() JavaDoc](http://web.archive.org/web/20221205123008/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#findAny--)

<input type="hidden" id="mkyong-current-postId" value="15387">