# Java 8 双消费示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-biconsumer-examples/>

在 Java 8 中， [BiConsumer](http://web.archive.org/web/20221205194609/https://docs.oracle.com/javase/8/docs/api/java/util/function/BiConsumer.html) 是一个函数接口；它接受两个参数，不返回任何内容。

```java
 @FunctionalInterface
public interface BiConsumer<T, U> {
  void accept(T t, U u);
} 
```

**延伸阅读**–[Java 8 消费者实例](/web/20221205194609/https://mkyong.com/java8/java-8-consumer-examples/)

## 1.双消费者

JavaBiConsumer1.java

```java
 package com.mkyong.java8;

import java.util.function.Consumer;

public class JavaBiConsumer1 {

    public static void main(String[] args) {

      BiConsumer<Integer, Integer> addTwo = (x, y) -> System.out.println(x + y);
      addTwo.accept(1, 2);    // 3

    }

} 
```

输出

```java
 3 
```

## 2.高阶函数

2.1 这个例子接受`BiConsumer`作为参数，创建一个泛型`addTwo`来连接两个对象。

JavaBiConsumer2.java

```java
 package com.mkyong.java8;

import java.util.function.BiConsumer;

public class JavaBiConsumer2 {

    public static void main(String[] args) {

        addTwo(1, 2, (x, y) -> System.out.println(x + y));          // 3
        addTwo("Node", ".js", (x, y) -> System.out.println(x + y)); // Node.js

    }

    static <T> void addTwo(T a1, T a2, BiConsumer<T, T> c) {
        c.accept(a1, a2);
    }

} 
```

输出

```java
 3
Node.js 
```

2.2 更多`BiConsumer`例子。

JavaBiConsumer3.java

```java
 package com.mkyong.java8;

import java.util.function.BiConsumer;

public class JavaBiConsumer3 {

    public static void main(String[] args) {

        math(1, 1, (x, y) -> System.out.println(x + y));   // 2
        math(1, 1, (x, y) -> System.out.println(x - y));   // 0
        math(1, 1, (x, y) -> System.out.println(x * y));   // 1
        math(1, 1, (x, y) -> System.out.println(x / y));   // 1

    }

    static <Integer> void math(Integer a1, Integer a2, BiConsumer<Integer, Integer> c) {
        c.accept(a1, a2);
    }

} 
```

输出

```java
 2
0
1
1 
```

## 3.Map.forEach

在 JDK 源代码中，`Map.forEach`接受一个`BiConsumer`作为参数。

Map.java

```java
 default void forEach(BiConsumer<? super K, ? super V> action) {
        Objects.requireNonNull(action);
        for (Map.Entry<K, V> entry : entrySet()) {
            K k;
            V v;
            try {
                k = entry.getKey();
                v = entry.getValue();
            } catch (IllegalStateException ise) {
                // this usually means the entry is no longer in the map.
                throw new ConcurrentModificationException(ise);
            }
            action.accept(k, v);
        }
    } 
```

JavaMapBiConsumer.java

```java
 package com.mkyong.java8;

import java.util.LinkedHashMap;
import java.util.Map;

public class JavaMapBiConsumer {

    public static void main(String[] args) {

        Map<Integer, String> map = new LinkedHashMap<>();

        map.put(1, "Java");
        map.put(2, "C++");
        map.put(3, "Rust");
        map.put(4, "JavaScript");
        map.put(5, "Go");

        map.forEach((k, v) -> System.out.println(k + ":" + v));

    }

} 
```

输出

```java
 1:Java
2:C++
3:Rust
4:JavaScript
5:Go 
```

## 参考

*   [双消费 JavaDoc](http://web.archive.org/web/20221205194609/https://docs.oracle.com/javase/8/docs/api/java/util/function/BiConsumer.html)
*   [Java 8 谓词示例](/web/20221205194609/https://mkyong.com/java8/java-8-predicate-examples/)
*   [Java 8 消费者示例](/web/20221205194609/https://mkyong.com/java8/java-8-consumer-examples/)

<input type="hidden" id="mkyong-current-postId" value="15423">