# Java 8 Stream.reduce()示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-stream-reduce-examples/>

在 Java 8 中，`Stream.reduce()`组合流中的元素并产生一个值。

使用 for 循环的简单求和运算。

```java
 int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  int sum = 0;
  for (int i : numbers) {
      sum += i;
  }

  System.out.println("sum : " + sum); // 55 
```

`Stream.reduce()`中的等价物

```java
 int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

  // 1st argument, init value = 0
  int sum = Arrays.stream(numbers).reduce(0, (a, b) -> a + b);

  System.out.println("sum : " + sum); // 55 
```

或者用`Integer::sum`引用方法

```java
 int sum = Arrays.stream(numbers).reduce(0, Integer::sum); // 55 
```

Integer.java

```java
 /**
     * Adds two integers together as per the + operator.
     *
     * @param a the first operand
     * @param b the second operand
     * @return the sum of {@code a} and {@code b}
     * @see java.util.function.BinaryOperator
     * @since 1.8
     */
    public static int sum(int a, int b) {
        return a + b;
    } 
```

## 1.方法签名

1.1 查看`Stream.reduce()`方法签名:

Stream.java

```java
 T reduce(T identity, BinaryOperator<T> accumulator); 
```

IntStream.java

```java
 int reduce(int identity, IntBinaryOperator op); 
```

LongStream.java

```java
 long reduce(int identity, LongBinaryOperator op); 
```

*   identity =默认值或初始值。
*   BinaryOperator =函数接口，取两个值并产生一个新值。

1.2 如果缺少`identity`参数，则没有默认值或初始值，它返回一个可选的。

Stream.java

```java
 Optional<T> reduce(BinaryOperator<T> accumulator); 
```

## 2.更多示例

2.1 数学运算。

```java
 int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

int sum = Arrays.stream(numbers).reduce(0, (a, b) -> a + b);    // 55
int sum2 = Arrays.stream(numbers).reduce(0, Integer::sum);      // 55

int sum3 = Arrays.stream(numbers).reduce(0, (a, b) -> a - b);   // -55
int sum4 = Arrays.stream(numbers).reduce(0, (a, b) -> a * b);   // 0, initial is 0, 0 * whatever = 0
int sum5 = Arrays.stream(numbers).reduce(0, (a, b) -> a / b);   // 0 
```

2.2 最大和最小。

```java
 int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

int max = Arrays.stream(numbers).reduce(0, (a, b) -> a > b ? a : b);  // 10
int max1 = Arrays.stream(numbers).reduce(0, Integer::max);            // 10

int min = Arrays.stream(numbers).reduce(0, (a, b) -> a < b ? a : b);  // 0
int min1 = Arrays.stream(numbers).reduce(0, Integer::min);            // 0 
```

2.3 连接字符串。

```java
 String[] strings = {"a", "b", "c", "d", "e"};

  // |a|b|c|d|e , the initial | join is not what we want
  String reduce = Arrays.stream(strings).reduce("", (a, b) -> a + "|" + b);

  // a|b|c|d|e, filter the initial "" empty string
  String reduce2 = Arrays.stream(strings).reduce("", (a, b) -> {
      if (!"".equals(a)) {
          return a + "|" + b;
      } else {
          return b;
      }
  });

  // a|b|c|d|e , better uses the Java 8 String.join :)
  String join = String.join("|", strings); 
```

## 3.映射和缩小

一个简单的映射和简化示例，用于从发票列表中对`BigDecimal`求和。

JavaReduce.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.Arrays;
import java.util.List;

public class JavaReduce {

    public static void main(String[] args) {

        List<Invoice> invoices = Arrays.asList(
                new Invoice("A01", BigDecimal.valueOf(9.99), BigDecimal.valueOf(1)),
                new Invoice("A02", BigDecimal.valueOf(19.99), BigDecimal.valueOf(1.5)),
                new Invoice("A03", BigDecimal.valueOf(4.99), BigDecimal.valueOf(2))
        );

        BigDecimal sum = invoices.stream()
                .map(x -> x.getQty().multiply(x.getPrice()))    // map
                .reduce(BigDecimal.ZERO, BigDecimal::add);      // reduce

        System.out.println(sum);    // 49.955
        System.out.println(sum.setScale(2, RoundingMode.HALF_UP));  // 49.96

    }

}

class Invoice {

    String invoiceNo;
    BigDecimal price;
    BigDecimal qty;

    // getters, stters n constructor
} 
```

输出

```java
 49.955
49.96 
```

## 参考

*   [还原 JavaDoc](http://web.archive.org/web/20221205173700/https://docs.oracle.com/javase/tutorial/collections/streams/reduction.html)
*   二元运算符 JavaDoc
*   [Java–如何用逗号连接列表字符串](/web/20221205173700/https://mkyong.com/java/java-how-to-join-list-string-with-commas/)
*   [Java 8–string joiner 示例](/web/20221205173700/https://mkyong.com/java8/java-8-stringjoiner-example/)

<input type="hidden" id="mkyong-current-postId" value="15476">