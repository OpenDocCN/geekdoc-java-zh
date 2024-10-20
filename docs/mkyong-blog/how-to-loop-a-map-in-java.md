# 如何在 Java 中循环地图

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-loop-a-map-in-java/>

这段代码片段向您展示了如何在 Java 中循环一个`Map`。

```java
 Map<String, String> map = new HashMap<>();
	map.put("1", "Jan");
	map.put("2", "Feb");
	map.put("3", "Mar");

	// classic way, loop a Map
	for (Map.Entry<String, String> entry : map.entrySet()) {
		System.out.println("Key : " + entry.getKey() + " Value : " + entry.getValue());
	}

	//Java 8 only, forEach and Lambda
	map.forEach((k,v)->System.out.println("Key : " + k + " Value : " + v)); 
```

输出

```java
 Key : 1 Value :Jan
    Key : 2 Value :Feb
    Key : 3 Value :Mar

    Key : 1 Value :Jan
    Key : 2 Value :Feb
    Key : 3 Value :Mar 
```

## 1.例子

不同的循环方式`Map`

LoopMap.java

```java
 package com.mkyong;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class LoopMap {

    public static void main(String[] args) {

        // initial a Map
        Map<String, String> map = new HashMap<>();
        map.put("1", "Jan");
        map.put("2", "Feb");
        map.put("3", "Mar");
        map.put("4", "Apr");
        map.put("5", "May");
        map.put("6", "Jun");

        // Standard classic way, recommend!
        System.out.println("\nExample 1...");
        for (Map.Entry<String, String> entry : map.entrySet()) {
            System.out.println("Key : " + entry.getKey() + " Value : " + entry.getValue());
        }

        // Java 8, forEach and Lambda. recommend!
        System.out.println("\nExample 2...");
        map.forEach((k, v) -> System.out.println("Key : " + k + " Value : " + v));

        // Map -> Set -> Iterator -> Map.Entry -> troublesome, don't use, just for fun
        System.out.println("\nExample 3...");
        Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<String, String> entry = iterator.next();
            System.out.println("Key : " + entry.getKey() + " Value :" + entry.getValue());
        }

        // weired, but works anyway, don't use, just for fun
        System.out.println("\nExample 4...");
        for (Object key : map.keySet()) {
            System.out.println("Key : " + key.toString() + " Value : " + map.get(key));
        }

    }

} 
```

输出

```java
 Example 1...
Key : 1 Value : Jan
Key : 2 Value : Feb
Key : 3 Value : Mar
Key : 4 Value : Apr
Key : 5 Value : May
Key : 6 Value : Jun

Example 2...
Key : 1 Value : Jan
Key : 2 Value : Feb
Key : 3 Value : Mar
Key : 4 Value : Apr
Key : 5 Value : May
Key : 6 Value : Jun

Example 3...
Key : 1 Value :Jan
Key : 2 Value :Feb
Key : 3 Value :Mar
Key : 4 Value :Apr
Key : 5 Value :May
Key : 6 Value :Jun

Example 4...
Key : 1 Value : Jan
Key : 2 Value : Feb
Key : 3 Value : Mar
Key : 4 Value : Apr
Key : 5 Value : May
Key : 6 Value : Jun 
```

## 2.地图过滤

在 Java 8 中，你可以将一个`Map`转换成一个`Stream`并像这样过滤它:

```java
 map.entrySet().stream()
        .filter(x -> "Jan".equals(x.getValue()))
        .forEach( x -> System.out.println("Key : " + x.getKey() + " Value : " + x.getValue())); 
```

输出

```java
 Key : 1 Value : Jan 
```

**Note**
More examples, please read this [Java 8 – Filter a Map examples](http://web.archive.org/web/20210818021018/https://www.mkyong.com/java8/java-8-filter-a-map-examples/)

## 参考

1.  [Java . util . iterator JavaDoc](http://web.archive.org/web/20210818021018/https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)
2.  [java.util.Map JavaDoc](http://web.archive.org/web/20210818021018/https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)
3.  [Java 8 forEach 示例](http://web.archive.org/web/20210818021018/http://www.mkyong.com/java8/java-8-foreach-examples/)
4.  [在 Java 8 中迭代集合](http://web.archive.org/web/20210818021018/http://www.javaworld.com/article/2461744/java-language/java-language-iterating-over-collections-in-java-8.html)

Tags : [java8](http://web.archive.org/web/20210818021018/https://mkyong.com/tag/java8/) [loop](http://web.archive.org/web/20210818021018/https://mkyong.com/tag/loop/) [loop map](http://web.archive.org/web/20210818021018/https://mkyong.com/tag/loop-map/) [map](http://web.archive.org/web/20210818021018/https://mkyong.com/tag/map/) [map filter](http://web.archive.org/web/20210818021018/https://mkyong.com/tag/map-filter/)<input type="hidden" id="mkyong-current-postId" value="5971">