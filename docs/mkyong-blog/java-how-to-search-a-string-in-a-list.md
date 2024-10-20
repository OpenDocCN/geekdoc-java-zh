# Java——如何在列表中搜索字符串？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-search-a-string-in-a-list/>

在 Java 中，我们可以将一个普通的循环和`.contains()`、`.startsWith()`或`.matches()`结合起来，在`ArrayList`中搜索一个字符串。

JavaExample1.java

```java
 package com.mkyong.test;

import java.util.ArrayList;
import java.util.List;

public class JavaExample1 {

    public static void main(String[] args) {

        List<String> list = new ArrayList<>();
        list.add("Java");
        list.add("Kotlin");
        list.add("Clojure");
        list.add("Groovy");
        list.add("Scala");

        List<String> result = new ArrayList<>();
        for (String s : list) {
            if (s.contains("Java")) {
                result.add(s);
            }

            /*
            if (s.startsWith("J")) {
                result.add(s);
            }
            */

            /* regex
            if (s.matches("(?i)j.*")) {
                result.add(s);
            }
            */
        }

        System.out.println(result);

    }

} 
```

输出

```java
 [Java] 
```

对于 Java 8，现在就简单多了。

JavaExample2.java

```java
 package com.mkyong.test;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class JavaExample2 {

    public static void main(String[] args) {

        List<String> list = new ArrayList<>();
        list.add("Java");
        list.add("Kotlin");
        list.add("Clojure");
        list.add("Groovy");
        list.add("Scala");

        List<String> result = list
                .stream()
                .filter(x -> x.contains("Java"))
                .collect(Collectors.toList());

        System.out.println(result);

    }

} 
```

输出

```java
 [Java] 
```

## 参考

*   [弦。匹配 JavaDoc](http://web.archive.org/web/20220924120442/https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#matches-java.lang.String-)
*   [弦。从 JavaDoc](http://web.archive.org/web/20220924120442/https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#startsWith-java.lang.String-) 开始
*   [弦。包含 JavaDoc](http://web.archive.org/web/20220924120442/https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#contains-java.lang.CharSequence-)
*   [Java 8 流过滤器示例](/web/20220924120442/https://mkyong.com/java8/java-8-streams-filter-examples/)
*   JVM 语言的维基百科列表

<input type="hidden" id="mkyong-current-postId" value="15222">