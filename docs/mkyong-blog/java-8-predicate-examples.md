# Java 8 谓词示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-predicate-examples/>

在 Java 8 中，[谓词](http://web.archive.org/web/20221205205342/https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)是一个函数接口，它接受一个参数并返回一个布尔值。通常，它用于对象集合的过滤器中。

```java
 @FunctionalInterface
public interface Predicate<T> {
  boolean test(T t);
} 
```

**延伸阅读**
[Java 8 双预测例题](/web/20221205205342/https://mkyong.com/java8/java-8-bipredicate-examples/)

## 1.过滤器()中的谓词

`filter()`接受谓词作为参数。

Java8Predicate.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Java8Predicate {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> collect = list.stream().filter(x -> x > 5).collect(Collectors.toList());

        System.out.println(collect); // [6, 7, 8, 9, 10]

    }

} 
```

输出

```java
 [6, 7, 8, 9, 10] 
```

Java8Predicate.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8Predicate {

    public static void main(String[] args) {

        Predicate<Integer> noGreaterThan5 =  x -> x > 5;

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> collect = list.stream()
                .filter(noGreaterThan5)
                .collect(Collectors.toList());

        System.out.println(collect); // [6, 7, 8, 9, 10]

    }

} 
```

输出

```java
 [6, 7, 8, 9, 10] 
```

## 2.谓词. and()

2.1 多重过滤器。

Java8Predicate2.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Java8Predicate2 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // multiple filters
        List<Integer> collect = list.stream()
                .filter(x -> x > 5 && x < 8).collect(Collectors.toList());

        System.out.println(collect);

    }

} 
```

输出

```java
 [6, 7] 
```

2.1 替换为`Predicate.and()`

Java8Predicate2.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8Predicate2 {

    public static void main(String[] args) {

        Predicate<Integer> noGreaterThan5 = x -> x > 5;
        Predicate<Integer> noLessThan8 = x -> x < 8;

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> collect = list.stream()
                .filter(noGreaterThan5.and(noLessThan8))
                .collect(Collectors.toList());

        System.out.println(collect);

    }

} 
```

输出

```java
 [6, 7] 
```

## 3.谓词. or()

Java8Predicate3.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8Predicate3 {

    public static void main(String[] args) {

        Predicate<String> lengthIs3 = x -> x.length() == 3;
        Predicate<String> startWithA = x -> x.startsWith("A");

        List<String> list = Arrays.asList("A", "AA", "AAA", "B", "BB", "BBB");

        List<String> collect = list.stream()
                .filter(lengthIs3.or(startWithA))
                .collect(Collectors.toList());

        System.out.println(collect);

    }

} 
```

输出

```java
 [A, AA, AAA, BBB] 
```

## 4.Predicate.negate()

查找不以“A”开头的所有元素。

Java8Predicate4.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8Predicate4 {

    public static void main(String[] args) {

        Predicate<String> startWithA = x -> x.startsWith("A");

        List<String> list = Arrays.asList("A", "AA", "AAA", "B", "BB", "BBB");

        List<String> collect = list.stream()
                .filter(startWithA.negate())
                .collect(Collectors.toList());

        System.out.println(collect);

    }

} 
```

输出

```java
 [B, BB, BBB] 
```

## 5.函数中的 Predicate.test()

函数中的谓词。

Java8Predicate5.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8Predicate5 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("A", "AA", "AAA", "B", "BB", "BBB");

        System.out.println(StringProcessor.filter(
                list, x -> x.startsWith("A")));                    // [A, AA, AAA]

        System.out.println(StringProcessor.filter(
                list, x -> x.startsWith("A") && x.length() == 3)); // [AAA]

    }

}

class StringProcessor {
    static List<String> filter(List<String> list, Predicate<String> predicate) {
        return list.stream().filter(predicate::test).collect(Collectors.toList());
    }
} 
```

输出

```java
 [A, AA, AAA]
[AAA] 
```

## 6.谓词链接

我们可以将谓词链接在一起。

Java8Predicate6.java

```java
 package com.mkyong.java8;

import java.util.function.Predicate;

public class Java8Predicate6 {

    public static void main(String[] args) {

        Predicate<String> startWithA = x -> x.startsWith("a");

        // start with "a" or "m"
        boolean result = startWithA.or(x -> x.startsWith("m")).test("mkyong");
        System.out.println(result);     // true

        // !(start with "a" and length is 3)
        boolean result2 = startWithA.and(x -> x.length() == 3).negate().test("abc");
        System.out.println(result2);    // false

    }

} 
```

输出

```java
 true
false 
```

## 7.宾语中的谓语

Hosting.java

```java
 package com.mkyong.java8;

public class Hosting {

    private int Id;
    private String name;
    private String url;

    public Hosting(int id, String name, String url) {
        Id = id;
        this.name = name;
        this.url = url;
    }

    //... getters and setters, toString()
} 
```

HostingRespository.java

```java
 package com.mkyong.java8;

import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class HostingRespository {

    public static List<Hosting> filterHosting(List<Hosting> hosting,
                                              Predicate<Hosting> predicate) {
        return hosting.stream()
                .filter(predicate)
                .collect(Collectors.toList());
    }

} 
```

Java8Predicate7.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;

public class Java8Predicate7 {

    public static void main(String[] args) {

        Hosting h1 = new Hosting(1, "amazon", "aws.amazon.com");
        Hosting h2 = new Hosting(2, "linode", "linode.com");
        Hosting h3 = new Hosting(3, "liquidweb", "liquidweb.com");
        Hosting h4 = new Hosting(4, "google", "google.com");

        List<Hosting> list = Arrays.asList(new Hosting[]{h1, h2, h3, h4});

        List<Hosting> result = HostingRespository.filterHosting(list, x -> x.getName().startsWith("g"));
        System.out.println("result : " + result);  // google

        List<Hosting> result2 = HostingRespository.filterHosting(list, isDeveloperFriendly());
        System.out.println("result2 : " + result2); // linode

    }

    public static Predicate<Hosting> isDeveloperFriendly() {
        return n -> n.getName().equals("linode");
    }
} 
```

输出

```java
 result : [Hosting{Id=4, name='google', url='google.com'}]
result2 : [Hosting{Id=2, name='linode', url='linode.com'}] 
```

完成了。

## 参考

*   [谓词 JavaDoc](http://web.archive.org/web/20221205205342/https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)
*   [Java 8 流过滤器示例](/web/20221205205342/https://mkyong.com/java8/java-8-streams-filter-examples/)
*   [Java 8 双预测示例](/web/20221205205342/https://mkyong.com/java8/java-8-bipredicate-examples/)

<input type="hidden" id="mkyong-current-postId" value="15391">