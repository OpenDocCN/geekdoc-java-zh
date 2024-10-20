# Java——如何将原始数组转换成列表

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-convert-a-primitive-array-to-list/>

将原始数组`int[]`转换为`List<Integer>`的代码片段:

```java
 int[] number = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

	List<Integer> list = new ArrayList<>();
	for (int i : number) {
    	list.add(i);
	} 
```

在 Java 8 中，您可以使用流 API 进行装箱和转换，如下所示:

```java
 List<Integer> list = Arrays.stream(number).boxed().collect(Collectors.toList()); 
```

## 1.经典例子

将原始数组转换为列表的完整示例。

ArrayExample1.java

```java
 package com.mkyong.array;

import java.util.ArrayList;
import java.util.List;

public class ArrayExample1 {

    public static void main(String[] args) {

        int[] number = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        List<Integer> list = convertIntArrayToList(number);
        System.out.println("list : " + list);

    }

    private static List<Integer> convertIntArrayToList(int[] input) {

        List<Integer> list = new ArrayList<>();
        for (int i : input) {
            list.add(i);
        }
        return list;

    }
} 
```

输出

```java
 list : [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 
```

**Note**
You can’t use the popular `Arrays.asList` to convert it directly, because boxing issue.

```java
 int[] number = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
	// No, the return type is not what we want
	List<int[]> ints = Arrays.asList(number); 
```

## 2.Java 8 流

完整的 Java 8 流示例，用于将原始数组转换为列表。

ArrayExample2.java

```java
 package com.mkyong.array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class ArrayExample2 {

    public static void main(String[] args) {

        int[] number = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        // IntStream.of or Arrays.stream, same output
        //List<Integer> list = IntStream.of(number).boxed().collect(Collectors.toList());

        List<Integer> list = Arrays.stream(number).boxed().collect(Collectors.toList());
        System.out.println("list : " + list);

    }
} 
```

输出

```java
 list : [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 
```

## 参考

1.  [IntStream JavaDoc](http://web.archive.org/web/20210816234508/https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html)
2.  [Java–将数组转换成流](http://web.archive.org/web/20210816234508/https://www.mkyong.com/java8/java-how-to-convert-array-to-stream/)

Tags : [array](http://web.archive.org/web/20210816234508/https://mkyong.com/tag/array/) [intstream](http://web.archive.org/web/20210816234508/https://mkyong.com/tag/intstream/) [java8](http://web.archive.org/web/20210816234508/https://mkyong.com/tag/java8/) [list](http://web.archive.org/web/20210816234508/https://mkyong.com/tag/list/) [primitive](http://web.archive.org/web/20210816234508/https://mkyong.com/tag/primitive/) [stream](http://web.archive.org/web/20210816234508/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="14087">