# Java–将数组转换为数组列表

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-convert-array-to-arraylist/>

在 Java 中，我们可以使用`new ArrayList<>(Arrays.asList(array))`将一个`Array`转换成一个`ArrayList`

ArrayExample1.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ArrayExample1 {

    public static void main(String[] args) {

        String[] str = {"A", "B", "C"};

        List<String> list = new ArrayList<>(Arrays.asList(str));

        list.add("D");

        list.forEach(x -> System.out.println(x));

    }
} 
```

输出

```java
 A
B
C
D 
```

`Arrays.asList`返回一个固定的或不可修改的列表，如果我们修改列表，`UnsupportedOperationException`将被抛出。

```java
 List<String> list = Arrays.asList(str); 				 // unmodifiable list

	list.add("D"); 		// java.lang.UnsupportedOperationException

	List<String> list2 = new ArrayList<>(Arrays.asList(str)); // modifiable list

	list2.add("D"); 	// ok 
```

## 1.Java 8 流

ArrayExample2.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class ArrayExample2 {

    public static void main(String[] args) {

        String[] str = {"A", "B", "C"};

        ArrayList<String> list1 = Stream.of(str).collect(Collectors.toCollection(ArrayList::new));
        list1.add("D");
        list1.forEach(x -> System.out.println(x));

        List<String> list2 = Stream.of(str).collect(Collectors.toList());
        list2.add("D");
        list2.forEach(x -> System.out.println(x));

    }
} 
```

输出

```java
 A
B
C
D
A
B
C
D 
```

## 2.Java 9–list . of()

*P.S `List.of`返回固定或不可修改列表*

ArrayExample3.java

```java
 package com.mkyong;

import java.util.ArrayList;
import java.util.List;

public class ArrayExample3 {

    public static void main(String[] args) {

        String[] str = {"A", "B", "C"};

        // Java 9
        List<String> list = new ArrayList<>(List.of("A", "B", "C"));

        list.add("D");

        list.forEach(x -> System.out.println(x));

    }
} 
```

输出

```java
 A
B
C
D 
```

## 参考

*   [Java List Java . lang . unsupportedoperationexception](/web/20221205211249/https://mkyong.com/java/java-list-java-lang-unsupportedoperationexception/)
*   [Java 9 列表](http://web.archive.org/web/20221205211249/https://docs.oracle.com/javase/9/docs/api/java/util/List.html#of--)

<input type="hidden" id="mkyong-current-postId" value="15069">