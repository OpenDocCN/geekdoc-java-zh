# Java 8 双功能示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-bifunction-examples/>

在 Java 8 中， [BiFunction](http://web.archive.org/web/20220801141255/https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html) 是一个函数接口；它接受两个参数并返回一个对象。

BiFunction.java

```java
 @FunctionalInterface
public interface BiFunction<T, U, R> {

      R apply(T t, U u);

} 
```

*   t–函数第一个参数的类型。
*   u–函数的第二个参数的类型。
*   r–函数结果的类型。

## 1.`BiFunction<Integer, Integer, Integer>`

1.1 本例采用两个`Integers`并返回一个`Integer`、`Double`或`List`

Java8BiFunction1.java

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.List;
import java.util.function.BiFunction;

public class Java8BiFunction1 {

    public static void main(String[] args) {

        // takes two Integers and return an Integer
        BiFunction<Integer, Integer, Integer> func = (x1, x2) -> x1 + x2;

        Integer result = func.apply(2, 3);

        System.out.println(result); // 5

        // take two Integers and return an Double
        BiFunction<Integer, Integer, Double> func2 = (x1, x2) -> Math.pow(x1, x2);

        Double result2 = func2.apply(2, 4);

        System.out.println(result2);    // 16.0

        // take two Integers and return a List<Integer>
        BiFunction<Integer, Integer, List<Integer>> func3 = (x1, x2) -> Arrays.asList(x1 + x2);

        List<Integer> result3 = func3.apply(2, 3);

        System.out.println(result3);

    }

} 
```

输出

```java
 5
16.0
[5] 
```

## 2.`BiFunction<Integer, Integer, Double>`

2.1 这个`BiFunction`取两个`Integer`返回一个`Double`，用`andThen()`用一个`Function`把它链接起来，把`Double`转换成`String`。

Java8BiFunction2a.java

```java
 package com.mkyong;

import java.util.function.BiFunction;
import java.util.function.Function;

public class Java8BiFunction2a {

    public static void main(String[] args) {

        // Math.pow(a1, a2) returns Double
        BiFunction<Integer, Integer, Double> func1 = (a1, a2) -> Math.pow(a1, a2);

        // takes Double, returns String
        Function<Double, String> func2 = (input) -> "Result : " + String.valueOf(input);

        String result = func1.andThen(func2).apply(2, 4);

        System.out.println(result);

    }

} 
```

输出

```java
 Result : 16.0 
```

2.2 这个例子将上面的程序转换成一个方法，该方法接受`BiFunction`和`Function`作为参数，并将它们链接在一起。

Java8BiFunction2b.java

```java
 package com.mkyong;

import java.util.function.BiFunction;
import java.util.function.Function;

public class Java8BiFunction2b {

    public static void main(String[] args) {

        String result = powToString(2, 4,
                (a1, a2) -> Math.pow(a1, a2),
                (r) -> "Result : " + String.valueOf(r));

        System.out.println(result); // Result : 16.0

    }

    public static <R> R powToString(Integer a1, Integer a2,
                                    BiFunction<Integer, Integer, Double> func,
                                    Function<Double, R> func2) {

        return func.andThen(func2).apply(a1, a2);

    }

} 
```

输出

```java
 Result : 16.0 
```

2.3 此示例将上述方法转换为泛型方法:

```java
 public static <A1, A2, R1, R2> R2 convert(A1 a1, A2 a2,
                                          BiFunction<A1, A2, R1> func,
                                          Function<R1, R2> func2) {

    return func.andThen(func2).apply(a1, a2);

} 
```

这个泛型方法有很多可能性，让我们看看:

Java8BiFunction2c.java

```java
 package com.mkyong;

import java.util.function.BiFunction;
import java.util.function.Function;

public class Java8BiFunction2c {

    public static void main(String[] args) {

        // Take two Integers, pow it into a Double, convert Double into a String.
        String result = convert(2, 4,
                (a1, a2) -> Math.pow(a1, a2),
                (r) -> "Pow : " + String.valueOf(r));

        System.out.println(result);     // Pow : 16.0

        // Take two Integers, multiply into an Integer, convert Integer into a String.
        String result2 = convert(2, 4,
                (a1, a2) -> a1 * a1,
                (r) -> "Multiply : " + String.valueOf(r));

        System.out.println(result2);    // Multiply : 4

        // Take two Strings, join both, join "cde"
        String result3 = convert("a", "b",
                (a1, a2) -> a1 + a2,
                (r) -> r + "cde");      // abcde

        System.out.println(result3);

        // Take two Strings, join both, convert it into an Integer
        Integer result4 = convert("100", "200",
                (a1, a2) -> a1 + a2,
                (r) -> Integer.valueOf(r));

        System.out.println(result4);    // 100200

    }

    public static <A1, A2, R1, R2> R2 convert(A1 a1, A2 a2,
                                              BiFunction<A1, A2, R1> func,
                                              Function<R1, R2> func2) {

        return func.andThen(func2).apply(a1, a2);

    }

} 
```

输出

```java
 Pow : 16.0
Multiply : 4
abcde
100200 
```

## 3.工厂

3.1 这个例子使用`BiFunction`创建一个对象，充当一个工厂模式。

Java8BiFunction3.java

```java
 package com.mkyong;

import java.util.function.BiFunction;

public class Java8BiFunction3 {

    public static void main(String[] args) {

        GPS obj = factory("40.741895", "-73.989308", GPS::new);
        System.out.println(obj);

    }

    public static <R extends GPS> R factory(String Latitude, String Longitude,
                                            BiFunction<String, String, R> func) {
        return func.apply(Latitude, Longitude);
    }

}

class GPS {

    String Latitude;
    String Longitude;

    public GPS(String latitude, String longitude) {
        Latitude = latitude;
        Longitude = longitude;
    }

    public String getLatitude() {
        return Latitude;
    }

    public void setLatitude(String latitude) {
        Latitude = latitude;
    }

    public String getLongitude() {
        return Longitude;
    }

    public void setLongitude(String longitude) {
        Longitude = longitude;
    }

    @Override
    public String toString() {
        return "GPS{" +
                "Latitude='" + Latitude + '\'' +
                ", Longitude='" + Longitude + '\'' +
                '}';
    }
} 
```

输出

```java
 GPS{Latitude='40.741895', Longitude='-73.989308'} 
```

`GPS::new`调用下面的构造函数，它接受两个参数并返回一个对象(GPS ),因此它与`BiFunction`签名匹配。

```java
 public GPS(String latitude, String longitude) {
        Latitude = latitude;
        Longitude = longitude;
    } 
```

## 4.更大的

4.1 通过一些条件过滤一个`List`。

Java8BiFunction4.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.BiFunction;

public class Java8BiFunction4 {

    public static void main(String[] args) {

        Java8BiFunction4 obj = new Java8BiFunction4();

        List<String> list = Arrays.asList("node", "c++", "java", "javascript");

        List<String> result = obj.filterList(list, 3, obj::filterByLength);

        System.out.println(result);   // [node, java, javascript]

        List<String> result1 = obj.filterList(list, 3, (l1, size) -> {
            if (l1.length() > size) {
                return l1;
            } else {
                return null;
            }
        });

        System.out.println(result1);  // [node, java, javascript]

        List<String> result2 = obj.filterList(list, "c", (l1, condition) -> {
            if (l1.startsWith(condition)) {
                return l1;
            } else {
                return null;
            }
        });

        System.out.println(result2);  // [c++]

        List<Integer> number = Arrays.asList(1, 2, 3, 4, 5);

        List<Integer> result3 = obj.filterList(number, 2, (l1, condition) -> {
            if (l1 % condition == 0) {
                return l1;
            } else {
                return null;
            }
        });

        System.out.println(result3);  // [2, 4]

    }

    public String filterByLength(String str, Integer size) {
        if (str.length() > size) {
            return str;
        } else {
            return null;
        }
    }

    public <T, U, R> List<R> filterList(List<T> list1, U condition,
                                        BiFunction<T, U, R> func) {

        List<R> result = new ArrayList<>();

        for (T t : list1) {
            R apply = func.apply(t, condition);
            if (apply != null) {
                result.add(apply);
            }
        }

        return result;

    }

} 
```

输出

```java
 [node, java, javascript]
[node, java, javascript]
[c++]
[2, 4] 
```

## 参考

*   [函数 JavaDoc](http://web.archive.org/web/20220801141255/https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html)
*   [双功能 JavaDoc](http://web.archive.org/web/20220801141255/https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html)
*   [Java 8 函数示例](http://web.archive.org/web/20220801141255/https://mkyong.com/java8/java-8-function-examples/)
*   [Java 8 教程](/web/20220801141255/https://mkyong.com/tutorials/java-8-tutorials/)
*   [Java 8 谓词示例](/web/20220801141255/https://mkyong.com/java8/java-8-predicate-examples/)
*   [Java 8 双预测示例](/web/20220801141255/https://mkyong.com/java8/java-8-bipredicate-examples/)
*   [Java 8 消费者示例](/web/20220801141255/https://mkyong.com/java8/java-8-consumer-examples/)
*   [Java 8 双消费示例](/web/20220801141255/https://mkyong.com/java8/java-8-biconsumer-examples/)

<input type="hidden" id="mkyong-current-postId" value="15492">