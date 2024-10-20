# Java 8–将列表转换为地图

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-convert-list-to-map/>

几个 Java 8 示例向您展示如何将对象的`List`转换为`Map`，以及如何处理重复的键。

Hosting.java

```java
 package com.mkyong.java8

public class Hosting {

    private int Id;
    private String name;
    private long websites;

    public Hosting(int id, String name, long websites) {
        Id = id;
        this.name = name;
        this.websites = websites;
    }

    //getters, setters and toString()
} 
```

## 1.要映射的列表–collectors . tomap()

创建一个`Hosting`对象的列表，并使用`Collectors.toMap`将其转换成地图。

TestListMap.java

```java
 package com.mkyong.java8

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class TestListMap {

    public static void main(String[] args) {

        List<Hosting> list = new ArrayList<>();
        list.add(new Hosting(1, "liquidweb.com", 80000));
        list.add(new Hosting(2, "linode.com", 90000));
        list.add(new Hosting(3, "digitalocean.com", 120000));
        list.add(new Hosting(4, "aws.amazon.com", 200000));
        list.add(new Hosting(5, "mkyong.com", 1));

        // key = id, value - websites
        Map<Integer, String> result1 = list.stream().collect(
                Collectors.toMap(Hosting::getId, Hosting::getName));

        System.out.println("Result 1 : " + result1);

        // key = name, value - websites
        Map<String, Long> result2 = list.stream().collect(
                Collectors.toMap(Hosting::getName, Hosting::getWebsites));

        System.out.println("Result 2 : " + result2);

        // Same with result1, just different syntax
        // key = id, value = name
        Map<Integer, String> result3 = list.stream().collect(
                Collectors.toMap(x -> x.getId(), x -> x.getName()));

        System.out.println("Result 3 : " + result3);
    }
} 
```

输出

```java
 Result 1 : {1=liquidweb.com, 2=linode.com, 3=digitalocean.com, 4=aws.amazon.com, 5=mkyong.com}
Result 2 : {liquidweb.com=80000, mkyong.com=1, digitalocean.com=120000, aws.amazon.com=200000, linode.com=90000}
Result 3 : {1=liquidweb.com, 2=linode.com, 3=digitalocean.com, 4=aws.amazon.com, 5=mkyong.com} 
```

## 2.要映射的列表-重复的键！

2.1 运行下面的代码，会抛出重复的键错误！

TestDuplicatedKey.java

```java
 package com.mkyong.java8;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class TestDuplicatedKey {

    public static void main(String[] args) {

        List<Hosting> list = new ArrayList<>();
        list.add(new Hosting(1, "liquidweb.com", 80000));
        list.add(new Hosting(2, "linode.com", 90000));
        list.add(new Hosting(3, "digitalocean.com", 120000));
        list.add(new Hosting(4, "aws.amazon.com", 200000));
        list.add(new Hosting(5, "mkyong.com", 1));

        list.add(new Hosting(6, "linode.com", 100000)); // new line

        // key = name, value - websites , but the key 'linode' is duplicated!?
        Map<String, Long> result1 = list.stream().collect(
                Collectors.toMap(Hosting::getName, Hosting::getWebsites));

        System.out.println("Result 1 : " + result1);

    }
} 
```

输出——下面的错误信息有点误导，它应该显示“linode”而不是键值。

```java
 Exception in thread "main" java.lang.IllegalStateException: Duplicate key 90000
	at java.util.stream.Collectors.lambda$throwingMerger$0(Collectors.java:133)
	at java.util.HashMap.merge(HashMap.java:1245)
	//... 
```

2.2 要解决上面的重复键问题，请像这样传入第三个 mergeFunction 参数:

```java
 Map<String, Long> result1 = list.stream().collect(
                Collectors.toMap(Hosting::getName, Hosting::getWebsites,
                        (oldValue, newValue) -> oldValue
                )
        ); 
```

输出

```java
 Result 1 : {..., aws.amazon.com=200000, linode.com=90000} 
```

**Note**
`(oldValue, newValue) -> oldValue` ==> If the key is duplicated, do you prefer oldKey or newKey?

3.3 尝试新值

```java
 Map<String, Long> result1 = list.stream().collect(
                Collectors.toMap(Hosting::getName, Hosting::getWebsites,
                        (oldValue, newValue) -> newvalue
                )
        ); 
```

输出

```java
 Result 1 : {..., aws.amazon.com=200000, linode.com=100000} 
```

## 3.要映射的列表–排序和收集

TestSortCollect.java

```java
 package com.mkyong.java8;

import java.util.*;
import java.util.stream.Collectors;

public class TestSortCollect {

    public static void main(String[] args) {

        List<Hosting> list = new ArrayList<>();
        list.add(new Hosting(1, "liquidweb.com", 80000));
        list.add(new Hosting(2, "linode.com", 90000));
        list.add(new Hosting(3, "digitalocean.com", 120000));
        list.add(new Hosting(4, "aws.amazon.com", 200000));
        list.add(new Hosting(5, "mkyong.com", 1));
        list.add(new Hosting(6, "linode.com", 100000));

        //example 1
        Map result1 = list.stream()
                .sorted(Comparator.comparingLong(Hosting::getWebsites).reversed())
                .collect(
                        Collectors.toMap(
                                Hosting::getName, Hosting::getWebsites, // key = name, value = websites
                                (oldValue, newValue) -> oldValue,       // if same key, take the old key
                                LinkedHashMap::new                      // returns a LinkedHashMap, keep order
                        ));

        System.out.println("Result 1 : " + result1);

    }
} 
```

输出

```java
 Result 1 : {aws.amazon.com=200000, digitalocean.com=120000, linode.com=100000, liquidweb.com=80000, mkyong.com=1} 
```

在上面的例子中，流在收集之前被排序，所以“linode.com=100000”变成了“oldValue”。

## 参考

1.  [Java 8 收集器 JavaDoc](http://web.archive.org/web/20220627140650/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
2.  [Java 8–如何对地图进行排序](http://web.archive.org/web/20220627140650/http://www.mkyong.com/java8/java-8-how-to-sort-a-map/)
3.  [Java 8 Lambda:比较器示例](http://web.archive.org/web/20220627140650/https://www.mkyong.com/java8/java-8-lambda-comparator-example/)

<input type="hidden" id="mkyong-current-postId" value="13959">