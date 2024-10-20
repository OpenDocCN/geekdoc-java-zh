# Java 8 消费者示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-consumer-examples/>

在 Java 8 中，[消费者](http://web.archive.org/web/20221207231739/https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html)是一个函数接口；它接受一个参数，不返回任何值。

```java
 @FunctionalInterface
public interface Consumer<T> {
  void accept(T t);
} 
```

## 1.消费者

Java8Consumer1.java

```java
 package com.mkyong.java8;

import java.util.function.Consumer;

public class Java8Consumer1 {

    public static void main(String[] args) {

        Consumer<String> print = x -> System.out.println(x);
        print.accept("java");   // java

    }

} 
```

输出

```java
 java 
```

## 2.高阶函数

2.1 这个例子接受`Consumer`作为参数，模拟一个`forEach`来打印列表中的每一项。

Java8Consumer2.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class Java8Consumer2 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);

        // implementation of the Consumer's accept methods.
        Consumer<Integer> consumer = (Integer x) -> System.out.println(x);
        forEach(list, consumer);

        // or call this directly
        forEach(list, (Integer x) -> System.out.println(x));

    }

    static <T> void forEach(List<T> list, Consumer<T> consumer) {
        for (T t : list) {
            consumer.accept(t);
        }
    }

} 
```

输出

```java
 1
2
3
4
5
1
2
3
4
5 
```

2.2 同样的`forEach`方法接受`Consumer`作为参数；这一次，我们将打印字符串的长度。

Java8Consumer3.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class Java8Consumer3 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("a", "ab", "abc");
        forEach(list, (String x) -> System.out.println(x.length()));

    }

    static <T> void forEach(List<T> list, Consumer<T> consumer) {
        for (T t : list) {
            consumer.accept(t);
        }
    }

} 
```

输出

```java
 1
2
3 
```

看到灵活性了吗？

## 参考

*   [消费者 JavaDoc](http://web.archive.org/web/20221207231739/https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html)
*   [Java 8 谓词示例](/web/20221207231739/https://mkyong.com/java8/java-8-predicate-examples/)

<input type="hidden" id="mkyong-current-postId" value="15413">