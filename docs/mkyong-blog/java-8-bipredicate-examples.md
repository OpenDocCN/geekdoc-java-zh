# Java 8 双预测示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-bipredicate-examples/>

在 Java 8 中， [BiPredicate](http://web.archive.org/web/20220801141237/https://docs.oracle.com/javase/8/docs/api/java/util/function/BiPredicate.html) 是一个函数接口，它接受两个参数并返回一个布尔值，基本上这个`BiPredicate`和`Predicate`是一样的，但是它需要两个参数进行测试。

```java
 @FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
} 
```

**延伸阅读**
[Java 8 谓词示例](/web/20220801141237/https://mkyong.com/java8/java-8-predicate-examples/)

## 1.双预测 Hello World。

如果字符串长度与提供的长度匹配。

JavaBiPredicate1.java

```java
 package com.mkyong.java8;

import java.util.function.BiPredicate;

public class JavaBiPredicate1 {

    public static void main(String[] args) {

        BiPredicate<String, Integer> filter = (x, y) -> {
            return x.length() == y;
        };

        boolean result = filter.test("mkyong", 6);
        System.out.println(result);  // true

        boolean result2 = filter.test("java", 10);
        System.out.println(result2); // false
    }

} 
```

输出

```java
 true
false 
```

## 2.双预测作为函数参数。

这个例子使用`BiPredicate`通过域名或威胁分值过滤坏域名。

JavaBiPredicate2.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.function.BiPredicate;
import java.util.stream.Collectors;

public class JavaBiPredicate2 {

    public static void main(String[] args) {

        List<Domain> domains = Arrays.asList(new Domain("google.com", 1),
                new Domain("i-am-spammer.com", 10),
                new Domain("mkyong.com", 0),
                new Domain("microsoft.com", 2));

        BiPredicate<String, Integer> bi = (domain, score) -> {
            return (domain.equalsIgnoreCase("google.com") || score == 0);
        };

        // if google or score == 0
        List<Domain> result = filterBadDomain(domains, bi);
        System.out.println(result); // google.com, mkyong.com

        //  if score == 0
        List<Domain> result2 = filterBadDomain(domains, (domain, score) -> score == 0);
        System.out.println(result2); // mkyong.com, microsoft.com

        // if start with i or score > 5
        List<Domain> result3 = filterBadDomain(domains, (domain, score) -> domain.startsWith("i") && score > 5);
        System.out.println(result3); // i-am-spammer.com

        // chaining with or
        List<Domain> result4 = filterBadDomain(domains, bi.or(
                (domain, x) -> domain.equalsIgnoreCase("microsoft.com"))
        );
        System.out.println(result4); // google.com, mkyong.com, microsoft.com

    }

    public static <T extends Domain> List<T> filterBadDomain(
            List<T> list, BiPredicate<String, Integer> biPredicate) {

        return list.stream()
                .filter(x -> biPredicate.test(x.getName(), x.getScore()))
                .collect(Collectors.toList());

    }
}

class Domain {

    String name;
    Integer score;

    public Domain(String name, Integer score) {
        this.name = name;
        this.score = score;
    }
    // getters , setters , toString
} 
```

输出

```java
 [Domain{name='google.com', score=1}, Domain{name='mkyong.com', score=0}]
[Domain{name='mkyong.com', score=0}]
[Domain{name='i-am-spammer.com', score=10}]
[Domain{name='google.com', score=1}, Domain{name='mkyong.com', score=0}, Domain{name='microsoft.com', score=2}] 
```

## 参考

*   [双预测 JavaDoc](http://web.archive.org/web/20220801141237/https://docs.oracle.com/javase/8/docs/api/java/util/function/BiPredicate.html)
*   [Java 8 方法参考:如何使用](http://web.archive.org/web/20220801141237/https://www.codementor.io/@eh3rrera/using-java-8-method-reference-du10866vx)
*   [Java 8 谓词示例](/web/20220801141237/https://mkyong.com/java8/java-8-predicate-examples/)

<input type="hidden" id="mkyong-current-postId" value="15400">