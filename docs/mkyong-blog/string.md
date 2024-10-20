# Java 8–将可选的<string>转换为字符串

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-optionalstring-to-string/>

在 Java 8 中，我们可以使用`.map(Object::toString)`将一个`Optional<String>`转换成一个`String`。

```java
 String result = list.stream()
              .filter(x -> x.length() == 1)
              .findFirst()  // returns Optional
              .map(Object::toString)
              .orElse(""); 
```

## 样品

获得一个值的标准方法。

Java8Example1.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Java8Example1 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("a", "b", "c", "d", "e");

        Optional<String> result = list.stream()
          .filter(x -> x.length() == 1)
          .findFirst();

        if (result.isPresent()) {
            System.out.println(result.get()); // a
        }

    }

} 
```

输出

```java
 a 
```

尝试`map(Object::toString)`

Java8Example2.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;

public class Java8Example2 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("a", "b", "c", "d", "e");

        String s = list.stream().filter(x -> x.length() == 1)
                .findFirst()
                .map(Object::toString)
                .orElse(null);

        System.out.println(s); // a
    }

} 
```

输出

```java
 a 
```

## 参考

*   [Java 8 流 findFirst()和 findAny()](/web/20221208025451/https://mkyong.com/java8/java-8-stream-findfirst-and-findany/)
*   可选的 JavaDoc

<input type="hidden" id="mkyong-current-postId" value="15457">