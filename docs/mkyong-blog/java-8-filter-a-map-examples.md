# Java 8–过滤地图示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-filter-a-map-examples/>

几个 Java 例子向你展示如何用 Java 8 stream API 过滤一个`Map`。

Java 8 之前:

```java
 Map<Integer, String> map = new HashMap<>();
    map.put(1, "linode.com");
    map.put(2, "heroku.com");

	String result = "";
	for (Map.Entry<Integer, String> entry : map.entrySet()) {
		if("something".equals(entry.getValue())){
			result = entry.getValue();
		}
	} 
```

使用 Java 8，你可以将一个`Map.entrySet()`转换成一个`stream`，然后是一个`filter()`和`collect()` it。

```java
 Map<Integer, String> map = new HashMap<>();
    map.put(1, "linode.com");
    map.put(2, "heroku.com");

	//Map -> Stream -> Filter -> String
	String result = map.entrySet().stream()
		.filter(x -> "something".equals(x.getValue()))
		.map(x->x.getValue())
		.collect(Collectors.joining());

	//Map -> Stream -> Filter -> MAP
	Map<Integer, String> collect = map.entrySet().stream()
		.filter(x -> x.getKey() == 2)
		.collect(Collectors.toMap(x -> x.getKey(), x -> x.getValue()));

	// or like this
	Map<Integer, String> collect = map.entrySet().stream()
		.filter(x -> x.getKey() == 3)
		.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue)); 
```

## 1.Java 8–过滤地图

按值过滤地图并返回字符串的完整示例。

TestMapFilter.java

```java
 package com.mkyong;

import java.util.HashMap;
import java.util.Map;
import java.util.stream.Collectors;

public class TestMapFilter {

    public static void main(String[] args) {

        Map<Integer, String> HOSTING = new HashMap<>();
        HOSTING.put(1, "linode.com");
        HOSTING.put(2, "heroku.com");
        HOSTING.put(3, "digitalocean.com");
        HOSTING.put(4, "aws.amazon.com");

        // Before Java 8
        String result = "";
        for (Map.Entry<Integer, String> entry : HOSTING.entrySet()) {
            if ("aws.amazon.com".equals(entry.getValue())) {
                result = entry.getValue();
            }
        }
        System.out.println("Before Java 8 : " + result);

        //Map -> Stream -> Filter -> String
        result = HOSTING.entrySet().stream()
                .filter(map -> "aws.amazon.com".equals(map.getValue()))
                .map(map -> map.getValue())
                .collect(Collectors.joining());

        System.out.println("With Java 8 : " + result);

        // filter more values
        result = HOSTING.entrySet().stream()
                .filter(x -> {
                    if (!x.getValue().contains("amazon") && !x.getValue().contains("digital")) {
                        return true;
                    }
                    return false;
                })
                .map(map -> map.getValue())
                .collect(Collectors.joining(","));

        System.out.println("With Java 8 : " + result);

    }

} 
```

输出

```java
 Before Java 8 : aws.amazon.com
With Java 8 : aws.amazon.com
With Java 8 : linode.com,heroku.com 
```

## 2.Java 8–过滤地图#2

另一个例子是通过键过滤一个`Map`，但是这次将返回一个`Map`

TestMapFilter2.java

```java
 package com.mkyong;

import java.util.HashMap;
import java.util.Map;
import java.util.stream.Collectors;

public class TestMapFilter2 {

    public static void main(String[] args) {

        Map<Integer, String> HOSTING = new HashMap<>();
        HOSTING.put(1, "linode.com");
        HOSTING.put(2, "heroku.com");
        HOSTING.put(3, "digitalocean.com");
        HOSTING.put(4, "aws.amazon.com");

        //Map -> Stream -> Filter -> Map
        Map<Integer, String> collect = HOSTING.entrySet().stream()
                .filter(map -> map.getKey() == 2)
                .collect(Collectors.toMap(p -> p.getKey(), p -> p.getValue()));

        System.out.println(collect); //output : {2=heroku.com}

        Map<Integer, String> collect2 = HOSTING.entrySet().stream()
                .filter(map -> map.getKey() <= 3)
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));

        System.out.println(collect2); //output : {1=linode.com, 2=heroku.com, 3=digitalocean.com}

    }

} 
```

输出

```java
 {2=heroku.com}
{1=linode.com, 2=heroku.com, 3=digitalocean.com} 
```

## 3.Java 8–过滤映射# 3–谓词

这一次，试试新的 Java 8 `Predicate`

TestMapFilter3.java

```java
 package com.mkyong;

import java.util.HashMap;
import java.util.Map;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class TestMapFilter3 {

	// Generic Map filterbyvalue, with predicate
    public static <K, V> Map<K, V> filterByValue(Map<K, V> map, Predicate<V> predicate) {
        return map.entrySet()
                .stream()
                .filter(x -> predicate.test(x.getValue()))
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
    }

    public static void main(String[] args) {

        Map<Integer, String> HOSTING = new HashMap<>();
        HOSTING.put(1, "linode.com");
        HOSTING.put(2, "heroku.com");
        HOSTING.put(3, "digitalocean.com");
        HOSTING.put(4, "aws.amazon.com");
        HOSTING.put(5, "aws2.amazon.com");

        //  {1=linode.com}
        Map<Integer, String> filteredMap = filterByValue(HOSTING, x -> x.contains("linode"));
        System.out.println(filteredMap);

        // {1=linode.com, 4=aws.amazon.com, 5=aws2.amazon.com}
        Map<Integer, String> filteredMap2 = filterByValue(HOSTING, x -> (x.contains("aws") || x.contains("linode")));
        System.out.println(filteredMap2);

        // {4=aws.amazon.com}
        Map<Integer, String> filteredMap3 = filterByValue(HOSTING, x -> (x.contains("aws") && !x.contains("aws2")));
        System.out.println(filteredMap3);

        // {1=linode.com, 2=heroku.com}
        Map<Integer, String> filteredMap4 = filterByValue(HOSTING, x -> (x.length() <= 10));
        System.out.println(filteredMap4);

    }

} 
```

输出

```java
 {1=linode.com}
{1=linode.com, 4=aws.amazon.com, 5=aws2.amazon.com}
{4=aws.amazon.com}
{1=linode.com, 2=heroku.com} 
```

## 参考

1.  [用 Java SE 8 流处理数据](http://web.archive.org/web/20210818174137/http://www.oracle.com/technetwork/articles/java/ma14-java-se-8-streams-2177646.html)
2.  [Java 收集器 JavaDoc](http://web.archive.org/web/20210818174137/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
3.  [Java 8 流过滤器示例](http://web.archive.org/web/20210818174137/http://www.mkyong.com/java8/java-8-streams-filter-examples/)

Tags : [java8](http://web.archive.org/web/20210818174137/https://mkyong.com/tag/java8/) [map](http://web.archive.org/web/20210818174137/https://mkyong.com/tag/map/) [map filter](http://web.archive.org/web/20210818174137/https://mkyong.com/tag/map-filter/) [predicate](http://web.archive.org/web/20210818174137/https://mkyong.com/tag/predicate/) [stream](http://web.archive.org/web/20210818174137/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="13956">