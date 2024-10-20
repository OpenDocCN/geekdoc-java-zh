# Java 8 二进制运算符示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-binaryoperator-examples/>

在 Java 8 中， [BinaryOperator](http://web.archive.org/web/20221128031305/https://docs.oracle.com/javase/8/docs/api/java/util/function/BinaryOperator.html) 是一个函数接口，它扩展了`BiFunction`。

`BinaryOperator`接受两个相同类型的参数，并返回相同类型参数的结果。

BinaryOperator.java

```java
 @FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
} 
```

`BiFunction`接受任意类型的两个参数，并返回任意类型的结果。

BiFunction.java

```java
 @FunctionalInterface
public interface BiFunction<T, U, R> {
      R apply(T t, U u);
} 
```

## 1.二元运算符

1.1 在本例中，接受并返回相同类型的`BiFunction<Integer, Integer, Integer>`可以替换为`BinaryOperator<Integer>`。

Java8BinaryOperator1.java

```java
 package com.mkyong;

import java.util.function.BiFunction;
import java.util.function.BinaryOperator;

public class Java8BinaryOperator1 {

    public static void main(String[] args) {

        // BiFunction
        BiFunction<Integer, Integer, Integer> func = (x1, x2) -> x1 + x2;

        Integer result = func.apply(2, 3);

        System.out.println(result); // 5

        // BinaryOperator
        BinaryOperator<Integer> func2 = (x1, x2) -> x1 + x2;

        Integer result2 = func.apply(2, 3);

        System.out.println(result2); // 5

    }

} 
```

输出

```java
 5
5 
```

## 2.`BinaryOperator<T>`作为参数

2.1 本例模拟一个`stream.reduce()`对所有`Integer`求和。

Java8BinaryOperator2.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;
import java.util.function.BinaryOperator;

public class Java8BinaryOperator2 {

    public static void main(String[] args) {

        Integer[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        Integer result = math(Arrays.asList(numbers), 0, (a, b) -> a + b);

        System.out.println(result); // 55

        Integer result2 = math(Arrays.asList(numbers), 0, Integer::sum);

        System.out.println(result2); // 55

    }

    public static <T> T math(List<T> list, T init, BinaryOperator<T> accumulator) {
        T result = init;
        for (T t : list) {
            result = accumulator.apply(result, t);
        }
        return result;
    }

} 
```

输出

```java
 55
55 
```

## 3.IntBinaryOperator

3.1 如果数学运算涉及到像`int`这样的基本类型，为了获得更好的性能，请更改为`IntBinaryOperator`。

Java8BinaryOperator3.java

```java
 package com.mkyong;

import java.util.function.IntBinaryOperator;
import java.util.stream.IntStream;

public class Java8BinaryOperator3 {

    public static void main(String[] args) {

        int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        int result = math((numbers), 0, (a, b) -> a + b);

        System.out.println(result); // 55

        int result2 = math((numbers), 0, Integer::sum);

        System.out.println(result2); // 55

        IntStream
    }

    public static int math(int[] list, int init, IntBinaryOperator accumulator) {
        int result = init;
        for (int t : list) {
            result = accumulator.applyAsInt(result, t);
        }
        return result;
    }

} 
```

输出

```java
 55
55 
```

## 4.BinaryOperator.maxBy()和 BinaryOperator.minBy()

4.1 这个例子使用了`BinaryOperator`和一个自定义的`Comparator`来从一个开发者列表中找到工资最高和最低的开发者。

Java8BinaryOperator4.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.function.BinaryOperator;

public class Java8BinaryOperator4 {

    public static void main(String[] args) {

        Developer dev1 = new Developer("jordan", BigDecimal.valueOf(9999));
        Developer dev2 = new Developer("jack", BigDecimal.valueOf(8888));
        Developer dev3 = new Developer("jaden", BigDecimal.valueOf(10000));
        Developer dev4 = new Developer("ali", BigDecimal.valueOf(2000));
        Developer dev5 = new Developer("mkyong", BigDecimal.valueOf(1));

        List<Developer> list = Arrays.asList(dev1, dev2, dev3, dev4, dev5);

        // 1\. Create a Comparator
        Comparator<Developer> comparing = Comparator.comparing(Developer::getSalary);

        // 2\. BinaryOperator with a custom Comparator
        BinaryOperator<Developer> bo = BinaryOperator.maxBy(comparing);

        Developer result = find(list, bo);

        System.out.println(result);     // Developer{name='jaden', salary=10000}

        // one line

        // find developer with highest pay
        Developer developer = find(list, BinaryOperator.maxBy(Comparator.comparing(Developer::getSalary)));
        System.out.println(developer);  // Developer{name='jaden', salary=10000}

        // find developer with lowest pay
        Developer developer2 = find(list, BinaryOperator.minBy(Comparator.comparing(Developer::getSalary)));
        System.out.println(developer2); // Developer{name='mkyong', salary=1}

    }

    public static Developer find(List<Developer> list, BinaryOperator<Developer> accumulator) {
        Developer result = null;
        for (Developer t : list) {
            if (result == null) {
                result = t;
            } else {
                result = accumulator.apply(result, t);
            }
        }
        return result;
    }

} 
```

Developer.java

```java
 package com.mkyong;

import java.math.BigDecimal;

public class Developer {

    String name;
    BigDecimal salary;

    public Developer(String name, BigDecimal salary) {
        this.name = name;
        this.salary = salary;
    }

    //...
} 
```

输出

```java
 Developer{name='jaden', salary=10000}
Developer{name='jaden', salary=10000}
Developer{name='mkyong', salary=1} 
```

## 参考

*   [二元运算器](http://web.archive.org/web/20221128031305/https://docs.oracle.com/javase/8/docs/api/java/util/function/BinaryOperator.html)
*   [Java 8 Lambda:比较器示例](/web/20221128031305/https://mkyong.com/java8/java-8-lambda-comparator-example/)
*   [Java 8 Stream.reduce()示例](http://web.archive.org/web/20221128031305/https://mkyong.com/java8/java-8-stream-reduce-examples/)
*   [双功能 JavaDoc](http://web.archive.org/web/20221128031305/https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html)
*   [Java 8 函数示例](http://web.archive.org/web/20221128031305/https://mkyong.com/java8/java-8-function-examples/)
*   [Java 8 教程](/web/20221128031305/https://mkyong.com/tutorials/java-8-tutorials/)

<input type="hidden" id="mkyong-current-postId" value="15509">