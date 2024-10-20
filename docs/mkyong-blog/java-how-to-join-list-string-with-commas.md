# Java–如何用逗号连接列表字符串

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-join-list-string-with-commas/>

在 Java 中，我们可以使用`String.join(",", list)`用逗号连接一个列表字符串。

## 1.Java 8

1.1 `String.join`

JavaStringExample1.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;

public class JavaStringExample1 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("a","b","c");
        String result = String.join(",", list);

        System.out.println(result);

    }

} 
```

输出

```java
 a,b,c 
```

1.2 流`Collectors.joining`

JavaStringExample2.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class JavaStringExample2 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("a", "b", "c");

        String result = list.stream().collect(Collectors.joining(","));

        System.out.println(result);

    }

} 
```

输出

```java
 a,b,c 
```

## 2.在过去

创建一个自定义方法来手动连接带分隔符的字符串。

JavaStringExample3.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;

public class JavaStringExample3 {

    public static void main(String[] args) {

        System.out.println(join(",", Arrays.asList("a")));
        System.out.println(join(",", Arrays.asList("a", "b")));
        System.out.println(join(",", Arrays.asList("a", "b", "c")));
        System.out.println(join(",", Arrays.asList("")));
        System.out.println(join(",", null));

    }

    private static String join(String separator, List<String> input) {

        if (input == null || input.size() <= 0) return "";

        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < input.size(); i++) {

            sb.append(input.get(i));

            // if not the last item
            if (i != input.size() - 1) {
                sb.append(separator);
            }

        }

        return sb.toString();

    }

} 
```

输出

```java
 a
a,b
a,b,c
//empty 
```

## 参考

1.  [Java 8–string joiner 示例](http://web.archive.org/web/20221020142707/https://www.mkyong.com/java8/java-8-stringjoiner-example/)

<input type="hidden" id="mkyong-current-postId" value="14846">