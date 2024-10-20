# 带索引的 Java 8 forEach 打印

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-foreach-print-with-index/>

一个简单的 Java 8 技巧来打印前面带有索引的`Array`或`List`。

## 1.带索引的数组

用`IntStream.range`生成索引。

JavaListWithIndex.java

```java
 package com.mkyong.java8;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class JavaArrayWithIndex {

    public static void main(String[] args) {

        String[] names = {"Java", "Node", "JavaScript", "Rust", "Go"};

        List<String> collect = IntStream.range(0, names.length)
                .mapToObj(index -> index + ":" + names[index])
                .collect(Collectors.toList());

        collect.forEach(System.out::println);

    }

} 
```

输出

```java
 0:Java
1:Node
2:JavaScript
3:Rust
4:Go 
```

## 2.带索引的列表

将`List`转换为`Map`，并使用`Map.size`作为索引。

Stream.java

```java
 <R> R collect(Supplier<R> supplier,
                  BiConsumer<R, ? super T> accumulator,
                  BiConsumer<R, R> combiner); 
```

JavaListWithIndex.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;

public class JavaListWithIndex {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("Java", "Node", "JavaScript", "Rust", "Go");

        HashMap<Integer, String> collect = list
                .stream()
                .collect(HashMap<Integer, String>::new,
                        (map, streamValue) -> map.put(map.size(), streamValue),
                        (map, map2) -> {
                        });

        collect.forEach((k, v) -> System.out.println(k + ":" + v));

    }

} 
```

输出

```java
 0:Java
1:Node
2:JavaScript
3:Rust
4:Go 
```

## 参考

*   [Stream.collect JavaDoc](http://web.archive.org/web/20220627140650/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#collect-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BiConsumer-)
*   [Java 8 双消费示例](/web/20220627140650/https://mkyong.com/java8/java-8-biconsumer-examples/)
*   [Java 8 forEach 示例](/web/20220627140650/https://mkyong.com/java8/java-8-foreach-examples/)

<input type="hidden" id="mkyong-current-postId" value="15426">