# Java–检查数组是否包含某个值？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-check-if-array-contains-a-certain-value/>

Java 示例检查数组(字符串或原始类型)是否包含特定值，用 Java 8 流 API 更新。

## 1.字符串数组

1.1 检查字符串数组是否包含某个值“A”。

StringArrayExample1.java

```java
 package com.mkyong.core;

import java.util.Arrays;
import java.util.List;

public class StringArrayExample1 {

    public static void main(String[] args) {

        String[] alphabet = new String[]{"A", "B", "C"};

        // Convert String Array to List
        List<String> list = Arrays.asList(alphabet);

        if(list.contains("A")){
            System.out.println("Hello A");
        }

    }

} 
```

输出

```java
 Hello A 
```

在 Java 8 中，您可以这样做:

```java
 // Convert to stream and test it
	boolean result = Arrays.stream(alphabet).anyMatch("A"::equals);
	if (result) {
		System.out.println("Hello A");
	} 
```

1.2 检查字符串数组是否包含多个值的示例:

StringArrayExample2.java

```java
 package com.mkyong.core;

import java.util.Arrays;
import java.util.List;

public class StringArrayExample2 {

    public static void main(String[] args) {

        String[] alphabet = new String[]{"A", "C"};

        // Convert String Array to List
        List<String> list = Arrays.asList(alphabet);

        // A or B
        if (list.contains("A") || list.contains("B")) {
            System.out.println("Hello A or B");
        }

        // A and B
        if (list.containsAll(Arrays.asList("A", "B"))) {
            System.out.println("Hello A and B");
        }

        // A and C
        if (list.containsAll(Arrays.asList("A", "C"))) {
            System.out.println("Hello A and C");
        }

    }

} 
```

输出

```java
 Hello A or B
Hello A and C 
```

## 2.原始数组

2.1 对于`int[]`这样的基元数组，需要手动循环测试条件:

PrimitiveArrayExample1.java

```java
 package com.mkyong.core;

import java.util.Arrays;
import java.util.List;

public class PrimitiveArrayExample1 {

    public static void main(String[] args) {

        int[] number = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        if(contains(number, 2)){
            System.out.println("Hello 2");
        }

    }

    public static boolean contains(final int[] array, final int v) {

        boolean result = false;

        for(int i : array){
            if(i == v){
                result = true;
                break;
            }
        }

        return result;
    }

} 
```

输出

```java
 Hello 2 
```

2.2 有了 Java 8，编码就简单多了~

ArrayExample1.java

```java
 package com.mkyong.core;

import java.util.stream.IntStream;
import java.util.stream.LongStream;

public class TestDate {

    public static void main(String[] args) {

        int[] number = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        //Java 8
        boolean result = IntStream.of(number).anyMatch(x -> x == 4);

        if (result) {
            System.out.println("Hello 4");
        } else {
            System.out.println("Where is number 4?");
        }

        long[] lNumber = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        boolean result2 = LongStream.of(lNumber).anyMatch(x -> x == 10);

        if (result2) {
            System.out.println("Hello 10");
        } else {
            System.out.println("Where is number 10?");
        }

    }

} 
```

输出

```java
 Hello 4
Hello 10 
```

**Note**
To check if a primitive array contains multiple values, convert [the array into a List](http://web.archive.org/web/20210814144453/http://www.mkyong.com/java/java-how-to-convert-a-primitive-array-to-list/) and compare it like example 1.2 above.

#### 引用

1.  [IntStream JavaDoc](http://web.archive.org/web/20210814144453/https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html)
2.  [阵列。aslist JavaDoc](http://web.archive.org/web/20210814144453/https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList(T...))

标签:[array](http://web.archive.org/web/20210814144453/https://mkyong.com/tag/array/)[IntStream](http://web.archive.org/web/20210814144453/https://mkyong.com/tag/intstream/)[java8](http://web.archive.org/web/20210814144453/https://mkyong.com/tag/java8/)[stream](http://web.archive.org/web/20210814144453/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="14089">