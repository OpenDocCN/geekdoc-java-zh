# Java 8 供应商示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-supplier-examples/>

在 Java 8 中，[供应商](http://web.archive.org/web/20221206011825/https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)是一个函数接口；它不接受任何参数并返回一个结果。

Supplier.java

```java
 @FunctionalInterface
public interface Supplier<T> {
    T get();
} 
```

## 1.供应商

1.1 本例使用`Supplier`返回当前日期时间。

Java8Supplier1.java

```java
 package com.mkyong;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.function.Supplier;

public class Java8Supplier1 {

    private static final DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

    public static void main(String[] args) {

        Supplier<LocalDateTime> s = () -> LocalDateTime.now();
        LocalDateTime time = s.get();

        System.out.println(time);

        Supplier<String> s1 = () -> dtf.format(LocalDateTime.now());
        String time2 = s1.get();

        System.out.println(time2);

    }

} 
```

输出

```java
 2020-03-02T16:10:49.281223
2020-03-02 16:10:49 
```

## 2.返回供应商

Java8Supplier2.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.List;
import java.util.function.Supplier;

public class Java8Supplier2<T> {

    public static void main(String[] args) {

        Java8Supplier2<String> obj = new Java8Supplier2();

        List<String> list = obj.supplier().get();

    }

    public Supplier<List<T>> supplier() {

        // lambda
        // return () -> new ArrayList<>();

        // constructor reference
        return ArrayList::new;

    }

} 
```

## 3.工厂

3.1 返回一个`Developer`对象的简单工厂方法。

Java8Supplier3.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.time.LocalDate;
import java.util.function.Supplier;

public class Java8Supplier3 {

    public static void main(String[] args) {

        Developer obj = factory(Developer::new);
        System.out.println(obj);

        Developer obj2 = factory(() -> new Developer("mkyong"));
        System.out.println(obj2);

    }

    public static Developer factory(Supplier<? extends Developer> s) {

        Developer developer = s.get();
        if (developer.getName() == null || "".equals(developer.getName())) {
            developer.setName("default");
        }
        developer.setSalary(BigDecimal.ONE);
        developer.setStart(LocalDate.of(2017, 8, 8));

        return developer;

    }

} 
```

Developer.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.time.LocalDate;

public class Developer {

    String name;
    BigDecimal salary;
    LocalDate start;

    // for factory(Developer::new);
    public Developer() {
    }

    // for factory(() -> new Developer("mkyong"));
    public Developer(String name) {
        this.name = name;
    }

    // get, set, constructor, toString
    //...

} 
```

输出

```java
 Developer{name='default', salary=1, start=2017-08-08}
Developer{name='mkyong', salary=1, start=2017-08-08} 
```

## 参考

*   [供应商](http://web.archive.org/web/20221206011825/https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)
*   [Java 8–如何格式化本地日期时间](/web/20221206011825/https://mkyong.com/java8/java-8-how-to-format-localdatetime/)
*   [DateTimeFormatter JavaDoc](http://web.archive.org/web/20221206011825/https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)
*   [Java 8 教程](/web/20221206011825/https://mkyong.com/tutorials/java-8-tutorials/)

<input type="hidden" id="mkyong-current-postId" value="15514">