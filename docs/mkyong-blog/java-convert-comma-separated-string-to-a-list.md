# Java–将逗号分隔的字符串转换为列表

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-convert-comma-separated-string-to-a-list/>

这个 Java 例子向你展示了如何将一个逗号分隔的字符串转换成一个列表，反之亦然。

## 1.要列出的逗号分隔的字符串

TestApp1.java

```java
 package com.mkyong.utils;

import java.util.Arrays;
import java.util.List;

public class TestApp1 {

    public static void main(String[] args) {

        String alpha = "A, B, C, D";

		//Remove whitespace and split by comma 
        List<String> result = Arrays.asList(alpha.split("\\s*,\\s*"));

        System.out.println(result);
    }

} 
```

输出

```java
 [A, B, C, D] 
```

## 2.列表为逗号分隔的字符串

不需要循环`List`，使用新的 Java 8 `String.join`

TestApp2.java

```java
 package com.mkyong.utils;

import java.util.Arrays;
import java.util.List;

public class TestApp2 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("A", "B", "C", "D");

        String result = String.join(",", list);
        System.out.println(result);
    }

} 
```

输出

```java
 A,B,C,D 
```

## 参考

1.  [StringJoiner JavaDoc](http://web.archive.org/web/20221206211041/https://docs.oracle.com/javase/8/docs/api/java/util/StringJoiner.html)
2.  [Java 8–string joiner 示例](http://web.archive.org/web/20221206211041/https://www.mkyong.com/java8/java-8-stringjoiner-example/)

<input type="hidden" id="mkyong-current-postId" value="14535">