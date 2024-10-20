# Java 8–将地图转换为列表

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-map-to-list/>

将一个`Map`转换成一个`List`的几个 Java 例子

```java
 Map<String, String> map = new HashMap<>();

// Convert all Map keys to a List
List<String> result = new ArrayList(map.keySet());

// Convert all Map values to a List
List<String> result2 = new ArrayList(map.values());

// Java 8, Convert all Map keys to a List
List<String> result3 = map.keySet().stream()
	.collect(Collectors.toList());

// Java 8, Convert all Map values  to a List
List<String> result4 = map.values().stream()
	.collect(Collectors.toList());

// Java 8, seem a bit long, but you can enjoy the Stream features like filter and etc. 
List<String> result5 = map.values().stream()
	.filter(x -> !"apple".equalsIgnoreCase(x))
	.collect(Collectors.toList());

// Java 8, split a map into 2 List, it works!
// refer example 3 below 
```

## 1.映射到列表

对于简单的`Map`到`List`的转换，只需使用下面的代码:

ConvertMapToList.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class ConvertMapToList {

    public static void main(String[] args) {

        Map<Integer, String> map = new HashMap<>();
        map.put(10, "apple");
        map.put(20, "orange");
        map.put(30, "banana");
        map.put(40, "watermelon");
        map.put(50, "dragonfruit");

        System.out.println("\n1\. Export Map Key to List...");

        List<Integer> result = new ArrayList(map.keySet());

        result.forEach(System.out::println);

        System.out.println("\n2\. Export Map Value to List...");

        List<String> result2 = new ArrayList(map.values());

        result2.forEach(System.out::println);

    }

} 
```

输出

```java
 1\. Export Map Key to List...
50
20
40
10
30

2\. Export Map Value to List...
dragonfruit
orange
watermelon
apple
banana 
```

 ## Java 8–映射到列表

对于 Java 8，您可以将`Map`转换成流，对其进行处理，并将其作为`List`返回

ConvertMapToList.java

```java
 package com.mkyong;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class ConvertMapToList {

    public static void main(String[] args) {

        Map<Integer, String> map = new HashMap<>();
        map.put(10, "apple");
        map.put(20, "orange");
        map.put(30, "banana");
        map.put(40, "watermelon");
        map.put(50, "dragonfruit");

        System.out.println("\n1\. Export Map Key to List...");

        List<Integer> result = map.keySet().stream()
                .collect(Collectors.toList());

        result.forEach(System.out::println);

        System.out.println("\n2\. Export Map Value to List...");

        List<String> result2 = map.values().stream()
                .collect(Collectors.toList());

        result2.forEach(System.out::println);

        System.out.println("\n3\. Export Map Value to List..., say no to banana");
        List<String> result3 = map.keySet().stream()
                .filter(x -> !"banana".equalsIgnoreCase(x))
                .collect(Collectors.toList());

        result3.forEach(System.out::println);

    }

} 
```

输出

```java
 1\. Export Map Key to List...
50
20
40
10
30

2\. Export Map Value to List...
dragonfruit
orange
watermelon
apple
banana

3\. Export Map Value to List..., say no to banana
dragonfruit
orange
watermelon
apple 
```

 ## 3.Java 8–将地图转换为 2 列表

这个例子有点极端，用`map.entrySet()`把一个`Map`转换成 2 个`List`

ConvertMapToList.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

//https://www.mkyong.com/java8/java-8-how-to-sort-a-map/
public class ConvertMapToList {

    public static void main(String[] args) {

        Map<Integer, String> map = new HashMap<>();
        map.put(10, "apple");
        map.put(20, "orange");
        map.put(30, "banana");
        map.put(40, "watermelon");
        map.put(50, "dragonfruit");

        // split a map into 2 List
        List<Integer> resultSortedKey = new ArrayList<>();
        List<String> resultValues = map.entrySet().stream()
                //sort a Map by key and stored in resultSortedKey
                .sorted(Map.Entry.<Integer, String>comparingByKey().reversed())
                .peek(e -> resultSortedKey.add(e.getKey()))
                .map(x -> x.getValue())
                // filter banana and return it to resultValues
                .filter(x -> !"banana".equalsIgnoreCase(x))
                .collect(Collectors.toList());

        resultSortedKey.forEach(System.out::println);
        resultValues.forEach(System.out::println);

    }

} 
```

输出

```java
 //resultSortedKey
50
40
30
20
10

//resultValues
dragonfruit
watermelon
orange
apple 
```

## 参考

1.  [Java 8–将列表转换为映射](http://web.archive.org/web/20190522022908/https://www.mkyong.com/java8/java-8-convert-list-to-map/)
2.  [Java 8 forEach 示例](http://web.archive.org/web/20190522022908/http://www.mkyong.com/java8/java-8-foreach-examples/)

[convert](http://web.archive.org/web/20190522022908/https://www.mkyong.com/tag/convert/) [java8](http://web.archive.org/web/20190522022908/https://www.mkyong.com/tag/java8/) [list](http://web.archive.org/web/20190522022908/https://www.mkyong.com/tag/list/) [map](http://web.archive.org/web/20190522022908/https://www.mkyong.com/tag/map/)







