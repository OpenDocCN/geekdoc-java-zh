# Java 8 流–将`List<List<String>>`转换为`List<String>`

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-8-stream-convert-listliststring-to-liststring/>

如题，我们可以用`flatMap`来转换。

Java9Example1.java

```java
 package com.mkyong.test;

import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

public class Java9Example1 {

    public static void main(String[] args) {

        List<String> numbers = Arrays.asList("1", "2", "A", "B", "C1D2E3");

        List<List<String>> collect = numbers.stream()
                .map(x -> new Scanner(x).findAll("\\D+")
                        .map(m -> m.group())
                        .collect(Collectors.toList())
                )
                .collect(Collectors.toList());

        collect.forEach(x -> System.out.println(x));

    }

} 
```

输出

```java
 []
[]
[A]
[B]
[C, D, E] 
```

## 解决办法

Java9Example2.java

```java
 package com.mkyong.test;

import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

public class Java9Example2 {

    public static void main(String[] args) {

        List<String> numbers = Arrays.asList("1", "2", "A", "B", "C1D2E3");

        List<String> collect = numbers.stream()
                .map(x -> new Scanner(x).findAll("\\D+")
                        .map(m -> m.group())
                        .collect(Collectors.toList())
                )									 	// List<List<String>>
                .flatMap(List::stream)					// List<String>
                .collect(Collectors.toList());

        collect.forEach(x -> System.out.println(x));

    }

} 
```

输出

```java
 A
B
C
D
E 
```

## 参考

*   [Java 正则表达式示例](/web/20221205230300/https://mkyong.com/java/java-regular-expression-examples/)
*   [Scanner.findAll JavaDocs](http://web.archive.org/web/20221205230300/https://docs.oracle.com/javase/9/docs/api/java/util/Scanner.html#findAll-java.lang.String-)

<input type="hidden" id="mkyong-current-postId" value="15146">