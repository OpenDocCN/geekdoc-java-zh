# Java 8 函数示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-function-examples/>

在 Java 8 中，[函数](http://web.archive.org/web/20221205162226/https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)是一个函数接口；它接受一个参数(T 类型的对象)并返回一个对象(R 类型的对象)。参数和输出可以是不同的类型。

Function.java

```java
 @FunctionalInterface
public interface Function<T, R> {

      R apply(T t);

} 
```

*   t–函数输入的类型。
*   r–函数结果的类型。

**Further Reading**
[Java 8 BiFunction Examples](/web/20221205162226/https://mkyong.com/java8/java-8-bifunction-examples/)

## 1.功能

1.1 本例采用一个`<T> String`并将字符串的长度返回为`<R> Integer`。

Java8Function1.java

```java
 package com.mkyong;

import java.util.function.Function;

public class JavaMoney {

    public static void main(String[] args) {

        Function<String, Integer> func = x -> x.length();

        Integer apply = func.apply("mkyong");   // 6

        System.out.println(apply);

    }

} 
```

输出

```java
 6 
```

## 2.连锁功能

2.1 本例将`Function`与`andThen()`链接起来。

Java8Function2.java

```java
 package com.mkyong;

import java.util.function.Function;

public class Java8Function2 {

    public static void main(String[] args) {

        Function<String, Integer> func = x -> x.length();

        Function<Integer, Integer> func2 = x -> x * 2;

        Integer result = func.andThen(func2).apply("mkyong");   // 12

        System.out.println(result);

    }

} 
```

输出

```java
 12 
```

## 3.列表->地图

3.1 这个例子接受`Function`作为参数，将一个`List`转换成一个`Map`。

Java8Function3.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.function.Function;

public class Java8Function3 {

    public static void main(String[] args) {

        Java8Function3 obj = new Java8Function3();

        List<String> list = Arrays.asList("node", "c++", "java", "javascript");

        // lambda
        Map<String, Integer> map = obj.convertListToMap(list, x -> x.length());

        System.out.println(map);    // {node=4, c++=3, java=4, javascript=10}

        // method reference
        Map<String, Integer> map2 = obj.convertListToMap(list, obj::getLength);

        System.out.println(map2);
    }

    public <T, R> Map<T, R> convertListToMap(List<T> list, Function<T, R> func) {

        Map<T, R> result = new HashMap<>();
        for (T t : list) {
            result.put(t, func.apply(t));
        }
        return result;

    }

    public Integer getLength(String str) {
        return str.length();
    }

} 
```

输出

```java
 {node=4, c++=3, java=4, javascript=10}
{node=4, c++=3, java=4, javascript=10} 
```

## 4.列表->列表

4.1 这个例子接受`Function`作为一个参数，将一个字符串的`List`转换成另一个字符串的`List`，用 SHA256 散列。

Java8Function4

```java
 package com.mkyong;

import org.apache.commons.codec.digest.DigestUtils;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class Java8Function4 {

    public static void main(String[] args) {

        Java8Function4 obj = new Java8Function4();

        List<String> list = Arrays.asList("node", "c++", "java", "javascript");

        // lambda
        //List<String> result = obj.map(list, x -> obj.sha256(x));

        // method reference
        List<String> result = obj.map(list, obj::sha256);

        result.forEach(System.out::println);

    }

    public <T, R> List<R> map(List<T> list, Function<T, R> func) {

        List<R> result = new ArrayList<>();
        for (T t : list) {
            result.add(func.apply(t));
        }
        return result;

    }

    // sha256 a string
    public String sha256(String str) {
        return DigestUtils.sha256Hex(str);
    }

} 
```

输出

```java
 545ea538461003efdc8c81c244531b003f6f26cfccf6c0073b3239fdedf49446
cedb1bac7efcd7db47e9f2f2250a7c832aba83b410dd85766e2aea6ec9321e51
38a0963a6364b09ad867aa9a66c6d009673c21e182015461da236ec361877f77
eda71746c01c3f465ffd02b6da15a6518e6fbc8f06f1ac525be193be5507069d 
```

*附注:`DigestUtils.sha256Hex`来自`commons-codec`。*

pom.xml

```java
 <dependency>
  <groupId>commons-codec</groupId>
  <artifactId>commons-codec</artifactId>
  <version>1.14</version>
</dependency> 
```

## 参考

*   [函数 JavaDoc](http://web.archive.org/web/20221205162226/https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)
*   [Java 8 教程](/web/20221205162226/https://mkyong.com/tutorials/java-8-tutorials/)
*   [Java 8 谓词示例](/web/20221205162226/https://mkyong.com/java8/java-8-predicate-examples/)
*   [Java 8 双预测示例](/web/20221205162226/https://mkyong.com/java8/java-8-bipredicate-examples/)
*   [Java 8 消费者示例](/web/20221205162226/https://mkyong.com/java8/java-8-consumer-examples/)
*   [Java 8 双消费示例](/web/20221205162226/https://mkyong.com/java8/java-8-biconsumer-examples/)

<input type="hidden" id="mkyong-current-postId" value="15487">