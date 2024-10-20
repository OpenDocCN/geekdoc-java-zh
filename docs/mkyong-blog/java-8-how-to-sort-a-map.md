# Java 8–如何对地图进行排序

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-sort-a-map/>

![sorting](img/a0c8c065140400fd7caf7b8b7cc84c89.png)

Java 8 `Stream`示例按照键或值对一个`Map`进行排序。

## 1.快速解释

Java 8 中对地图进行排序的步骤。

1.  将地图转换成河流
2.  分类吧
3.  收集并返回新的`LinkedHashMap`(保持顺序)

```java
 Map result = map.entrySet().stream()
	.sorted(Map.Entry.comparingByKey()) 			
	.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,
	(oldValue, newValue) -> oldValue, LinkedHashMap::new)); 
```

*P.S 默认情况下，`Collectors.toMap`会返回一个`HashMap`*

## 2.按关键字排序

SortByKeyExample.java

```java
 package com.mkyong.test;

import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.stream.Collectors;

public class SortByKeyExample {

    public static void main(String[] argv) {

        Map<String, Integer> unsortMap = new HashMap<>();
        unsortMap.put("z", 10);
        unsortMap.put("b", 5);
        unsortMap.put("a", 6);
        unsortMap.put("c", 20);
        unsortMap.put("d", 1);
        unsortMap.put("e", 7);
        unsortMap.put("y", 8);
        unsortMap.put("n", 99);
        unsortMap.put("g", 50);
        unsortMap.put("m", 2);
        unsortMap.put("f", 9);

        System.out.println("Original...");
        System.out.println(unsortMap);

        // sort by keys, a,b,c..., and return a new LinkedHashMap
        // toMap() will returns HashMap by default, we need LinkedHashMap to keep the order.
        Map<String, Integer> result = unsortMap.entrySet().stream()
                .sorted(Map.Entry.comparingByKey())
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,
                        (oldValue, newValue) -> oldValue, LinkedHashMap::new));

        // Not Recommend, but it works.
        //Alternative way to sort a Map by keys, and put it into the "result" map
        Map<String, Integer> result2 = new LinkedHashMap<>();
        unsortMap.entrySet().stream()
                .sorted(Map.Entry.comparingByKey())
                .forEachOrdered(x -> result2.put(x.getKey(), x.getValue()));

        System.out.println("Sorted...");
        System.out.println(result);
        System.out.println(result2);

    }

} 
```

输出

```java
 Original...
{a=6, b=5, c=20, d=1, e=7, f=9, g=50, y=8, z=10, m=2, n=99}

Sorted...
{a=6, b=5, c=20, d=1, e=7, f=9, g=50, m=2, n=99, y=8, z=10}
{a=6, b=5, c=20, d=1, e=7, f=9, g=50, m=2, n=99, y=8, z=10} 
```

## 3.按值排序

SortByValueExample.java

```java
 package com.mkyong.test;

package com.mkyong;

import java.util.Comparator;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.stream.Collectors;

public class SortByValueExample {

    public static void main(String[] argv) {

        Map<String, Integer> unsortMap = new HashMap<>();
        unsortMap.put("z", 10);
        unsortMap.put("b", 5);
        unsortMap.put("a", 6);
        unsortMap.put("c", 20);
        unsortMap.put("d", 1);
        unsortMap.put("e", 7);
        unsortMap.put("y", 8);
        unsortMap.put("n", 99);
        unsortMap.put("g", 50);
        unsortMap.put("m", 2);
        unsortMap.put("f", 9);

        System.out.println("Original...");
        System.out.println(unsortMap);

        //sort by values, and reserve it, 10,9,8,7,6...
        Map<String, Integer> result = unsortMap.entrySet().stream()
                .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,
                        (oldValue, newValue) -> oldValue, LinkedHashMap::new));

        //Alternative way
        Map<String, Integer> result2 = new LinkedHashMap<>();
        unsortMap.entrySet().stream()
                .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
                .forEachOrdered(x -> result2.put(x.getKey(), x.getValue()));

        System.out.println("Sorted...");
        System.out.println(result);
        System.out.println(result2);

    }
} 
```

输出

```java
 Original...
{a=6, b=5, c=20, d=1, e=7, f=9, g=50, y=8, z=10, m=2, n=99}

Sorted...
{n=99, g=50, c=20, z=10, f=9, y=8, e=7, a=6, b=5, m=2, d=1}
{n=99, g=50, c=20, z=10, f=9, y=8, e=7, a=6, b=5, m=2, d=1} 
```

## 4.地图 <object>流不能直接对`Map<Object,Object>`排序。要解决它，把它转换成`Map<String,String>`，回顾下面的例子。

**Note**
If you have better idea to sort it, do let me know.DisplayApp.java

```java
 package com.mkyong;

import java.util.LinkedHashMap;
import java.util.Map;
import java.util.Properties;
import java.util.Set;
import java.util.stream.Collectors;

public class DisplayApp {

    public static void main(String[] args) {

        Properties properties = System.getProperties();
        // not easy to sort this
        Set<Map.Entry<Object, Object>> entries = properties.entrySet();

        LinkedHashMap<String, String> collect = entries.stream()
                //Map<String, String>
                .collect(Collectors.toMap(k -> (String) k.getKey(), e -> (String) e.getValue()))
                .entrySet()
                .stream()
                .sorted(Map.Entry.comparingByKey())
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,
                        (oldValue, newValue) -> oldValue, LinkedHashMap::new));

        collect.forEach((k, v) -> System.out.println(k + ":" + v));

    }

} 
```

**Note**
Not using Java 8? Try this classic way to [sort a Map in Java](http://web.archive.org/web/20220627140652/https://www.mkyong.com/java/how-to-sort-a-map-in-java/).

## 参考

1.  [如何在 Java 中对地图进行排序](http://web.archive.org/web/20220627140652/https://www.mkyong.com/java/how-to-sort-a-map-in-java/)
2.  [LinkedHashMap JavaDoc](http://web.archive.org/web/20220627140652/https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html)
3.  [collectors . tomap()JavaDoc](http://web.archive.org/web/20220627140652/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toMap-java.util.function.Function-java.util.function.Function-)
4.  [通配符带来更多乐趣](http://web.archive.org/web/20220627140652/https://docs.oracle.com/javase/tutorial/extra/generics/morefun.html)
5.  [Collections.sort()签名](http://web.archive.org/web/20220627140652/https://docs.oracle.com/javase/6/docs/api/java/util/Collections.html#sort%28java.util.List%29)
6.  [Java 中超级 T >和扩展 T >的区别](http://web.archive.org/web/20220627140652/https://stackoverflow.com/questions/4343202/difference-between-super-t-and-extends-t-in-java)

<input type="hidden" id="mkyong-current-postId" value="14038"></object>