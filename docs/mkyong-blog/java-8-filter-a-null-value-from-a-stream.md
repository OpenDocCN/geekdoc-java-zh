# Java 8–从流中过滤空值

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-filter-a-null-value-from-a-stream/>

查看包含`null`值的`Stream`。

Java8Examples.java

```java
 package com.mkyong.java8;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Java8Examples {

    public static void main(String[] args) {

        Stream<String> language = Stream.of("java", "python", "node", null, "ruby", null, "php");

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
null   // <--- NULL
ruby
null   // <--- NULL
php

```

## 解决办法

为了解决这个问题，使用了`Stream.filter(x -> x!=null)`

Java8Examples.java

```java
 package com.mkyong.java8;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Java8Examples {

    public static void main(String[] args) {

        Stream<String> language = Stream.of("java", "python", "node", null, "ruby", null, "php");

        //List<String> result = language.collect(Collectors.toList());

        List<String> result = language.filter(x -> x!=null).collect(Collectors.toList());

        result.forEach(System.out::println);

    }
} 
```

输出

```java
java
python
node
ruby
php

```

或者，用`Objects::nonNull`过滤

```java
 import java.util.List;

	List<String> result = language.filter(Objects::nonNull).collect(Collectors.toList()); 
```

## 参考

1.  [Objects::非空 JavaDoc](http://web.archive.org/web/20220204092020/https://docs.oracle.com/javase/8/docs/api/java/util/Objects.html#nonNull-java.lang.Object-)
2.  [Java 8 流过滤器示例](http://web.archive.org/web/20220204092020/https://www.mkyong.com/java8/java-8-streams-filter-examples/)
3.  [Java 8 收集器 JavaDoc](http://web.archive.org/web/20220204092020/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)

<input type="hidden" id="mkyong-current-postId" value="14036">