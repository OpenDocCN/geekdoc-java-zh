# Java 8 一元运算符示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-unaryoperator-examples/>

在 Java 8 中，[一元运算符](http://web.archive.org/web/20221127133729/https://docs.oracle.com/javase/8/docs/api/java/util/function/UnaryOperator.html)是一个函数接口，它扩展了`Function`。

`UnaryOperator`接受一个参数，并返回相同类型参数的结果。

UnaryOperator.java

```java
 @FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {
} 
```

`Function`接受任意类型的一个参数并返回任意类型的结果。

Function.java

```java
 @FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
} 
```

**进一步阅读** [Java 8 函数示例](/web/20221127133729/https://mkyong.com/java8/java-8-function-examples/)

## 1.一元运算符

1.1 在本例中，接受并返回相同类型的`Function<Integer, Integer>`可以替换为`UnaryOperator<Integer>`。

Java8UnaryOperator1.java

```java
 package com.mkyong;

import java.util.function.Function;
import java.util.function.UnaryOperator;

public class Java8UnaryOperator1 {

    public static void main(String[] args) {

        Function<Integer, Integer> func = x -> x * 2;

        Integer result = func.apply(2);

        System.out.println(result);         // 4

        UnaryOperator<Integer> func2 = x -> x * 2;

        Integer result2 = func2.apply(2);

        System.out.println(result2);        // 4

    }

} 
```

输出

```java
 4
4 
```

## 2.`UnaryOperator<T>`作为参数

Java8UnaryOperator2.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.UnaryOperator;

public class Java8UnaryOperator2 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> result = math(list, x -> x * 2);

        System.out.println(result); // [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

    }

    public static <T> List<T> math(List<T> list, UnaryOperator<T> uo) {
        List<T> result = new ArrayList<>();
        for (T t : list) {
            result.add(uo.apply(t));
        }
        return result;
    }

} 
```

输出

```java
 [2, 4, 6, 8, 10, 12, 14, 16, 18, 20] 
```

## 3.链式单运算器

Java8UnaryOperator3.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.UnaryOperator;

public class Java8UnaryOperator3 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> result = math(list,
                x -> x * 2,
                x -> x + 1);

        System.out.println(result); // [3, 5, 7, 9, 11, 13, 15, 17, 19, 21]

    }

    public static <T> List<T> math(List<T> list,
                                   UnaryOperator<T> uo, UnaryOperator<T> uo2) {
        List<T> result = new ArrayList<>();
        for (T t : list) {
            result.add(uo.andThen(uo2).apply(t));
        }
        return result;
    }

} 
```

输出

```java
 [3, 5, 7, 9, 11, 13, 15, 17, 19, 21] 
```

## 参考

*   一元运算符 JavaDoc
*   [函数 JavaDoc](http://web.archive.org/web/20221127133729/https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)
*   [Java 8 函数示例](http://web.archive.org/web/20221127133729/https://mkyong.com/java8/java-8-function-examples/)
*   [Java 8 教程](/web/20221127133729/https://mkyong.com/tutorials/java-8-tutorials/)

<input type="hidden" id="mkyong-current-postId" value="15512">