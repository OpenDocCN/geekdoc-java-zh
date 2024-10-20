# Java 8–流收集器按示例分组

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-collectors-groupingby-and-mapping-example/>

在本文中，我们将向您展示如何使用 Java 8 Stream `Collectors`对一个`List`进行分组、计数、求和以及排序。

## 1.分组依据、计数和排序

1.1 按 a `List`分组并显示其总数。

Java8Example1.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Java8Example1 {

    public static void main(String[] args) {

        //3 apple, 2 banana, others 1
        List<String> items =
                Arrays.asList("apple", "apple", "banana",
                        "apple", "orange", "banana", "papaya");

        Map<String, Long> result =
                items.stream().collect(
                        Collectors.groupingBy(
                                Function.identity(), Collectors.counting()
                        )
                );

        System.out.println(result);

    }
} 
```

输出

```java
{
	papaya=1, orange=1, banana=2, apple=3
}

```

1.2 添加排序。

Java8Example2.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Java8Example2 {

    public static void main(String[] args) {

        //3 apple, 2 banana, others 1
        List<String> items =
                Arrays.asList("apple", "apple", "banana",
                        "apple", "orange", "banana", "papaya");

        Map<String, Long> result =
                items.stream().collect(
                        Collectors.groupingBy(
                                Function.identity(), Collectors.counting()
                        )
                );

        Map<String, Long> finalMap = new LinkedHashMap<>();

        //Sort a map and add to finalMap
        result.entrySet().stream()
                .sorted(Map.Entry.<String, Long>comparingByValue()
                        .reversed()).forEachOrdered(e -> finalMap.put(e.getKey(), e.getValue()));

        System.out.println(finalMap);

    }
} 
```

输出

```java
{
	apple=3, banana=2, papaya=1, orange=1
}

```

## 2.列出对象

用户定义对象列表的“分组依据”示例。

2.1 A Pojo。

Item.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;

public class Item {

    private String name;
    private int qty;
    private BigDecimal price;

    //constructors, getter/setters 
} 
```

2.2 按名称分组+计数或合计数量。

Java8Examples3.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class Java8Examples3 {

    public static void main(String[] args) {

        //3 apple, 2 banana, others 1
        List<Item> items = Arrays.asList(
                new Item("apple", 10, new BigDecimal("9.99")),
                new Item("banana", 20, new BigDecimal("19.99")),
                new Item("orang", 10, new BigDecimal("29.99")),
                new Item("watermelon", 10, new BigDecimal("29.99")),
                new Item("papaya", 20, new BigDecimal("9.99")),
                new Item("apple", 10, new BigDecimal("9.99")),
                new Item("banana", 10, new BigDecimal("19.99")),
                new Item("apple", 20, new BigDecimal("9.99"))
        );

        Map<String, Long> counting = items.stream().collect(
                Collectors.groupingBy(Item::getName, Collectors.counting()));

        System.out.println(counting);

        Map<String, Integer> sum = items.stream().collect(
                Collectors.groupingBy(Item::getName, Collectors.summingInt(Item::getQty)));

        System.out.println(sum);

    }
} 
```

输出

```java
//Group by + Count
{
	papaya=1, banana=2, apple=3, orang=1, watermelon=1
}

//Group by + Sum qty
{
	papaya=20, banana=30, apple=40, orang=10, watermelon=10
}

```

2.2 按价格分组—`Collectors.groupingBy`和`Collectors.mapping`示例。

Java8Examples4.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.stream.Collectors;

public class Java8Examples4 {

    public static void main(String[] args) {

        //3 apple, 2 banana, others 1
        List<Item> items = Arrays.asList(
                new Item("apple", 10, new BigDecimal("9.99")),
                new Item("banana", 20, new BigDecimal("19.99")),
                new Item("orang", 10, new BigDecimal("29.99")),
                new Item("watermelon", 10, new BigDecimal("29.99")),
                new Item("papaya", 20, new BigDecimal("9.99")),
                new Item("apple", 10, new BigDecimal("9.99")),
                new Item("banana", 10, new BigDecimal("19.99")),
                new Item("apple", 20, new BigDecimal("9.99"))
                );

		//group by price
        Map<BigDecimal, List<Item>> groupByPriceMap = 
			items.stream().collect(Collectors.groupingBy(Item::getPrice));

        System.out.println(groupByPriceMap);

		// group by price, uses 'mapping' to convert List<Item> to Set<String>
        Map<BigDecimal, Set<String>> result =
                items.stream().collect(
                        Collectors.groupingBy(Item::getPrice,
                                Collectors.mapping(Item::getName, Collectors.toSet())
                        )
                );

        System.out.println(result);

    }
} 
```

输出

```java
{
	19.99=[
			Item{name='banana', qty=20, price=19.99}, 
			Item{name='banana', qty=10, price=19.99}
		], 
	29.99=[
			Item{name='orang', qty=10, price=29.99}, 
			Item{name='watermelon', qty=10, price=29.99}
		], 
	9.99=[
			Item{name='apple', qty=10, price=9.99}, 
			Item{name='papaya', qty=20, price=9.99}, 
			Item{name='apple', qty=10, price=9.99}, 
			Item{name='apple', qty=20, price=9.99}
		]
}

//group by + mapping to Set
{
	19.99=[banana], 
	29.99=[orang, watermelon], 
	9.99=[papaya, apple]
}

```

## 参考

1.  [Java 8 流收集器 JavaDoc](http://web.archive.org/web/20210817062446/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
2.  [Java–如何对地图进行排序](http://web.archive.org/web/20210817062446/https://www.mkyong.com/java/how-to-sort-a-map-in-java/)
3.  [stack overflow–按值排序映射`<key value>`(Java)](http://web.archive.org/web/20210817062446/https://stackoverflow.com/questions/109383/sort-a-mapkey-value-by-values-java)

Tags : [collectors](http://web.archive.org/web/20210817062446/https://mkyong.com/tag/collectors/) [group by](http://web.archive.org/web/20210817062446/https://mkyong.com/tag/group-by/) [java8](http://web.archive.org/web/20210817062446/https://mkyong.com/tag/java8/) [stream](http://web.archive.org/web/20210817062446/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="13968">