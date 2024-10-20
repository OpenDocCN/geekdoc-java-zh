# Java——如何连接数组

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-join-arrays/>

在本文中，我们将向您展示几种加入 Java 数组的方法。

1.  Apache Commons Lang–ArrayUtils
2.  Java API
3.  Java 8 流

## 1.Apache Commons Lang–ArrayUtils

最简单的方法是添加 [Apache Commons Lang](http://web.archive.org/web/20210819013002/https://commons.apache.org/proper/commons-lang/javadocs/api-3.1/org/apache/commons/lang3/ArrayUtils.html#addAll%28T%5B%5D,%20T...%29) 库，并使用`ArrayUtils. addAll`加入数组。此方法支持基元和对象类型数组。

JoinArray.java

```java
 package com.mkyong.example.array;

import org.apache.commons.lang3.ArrayUtils;

import java.util.Arrays;

public class JoinArray {

    public static void main(String[] args) {

        String[] s1 = new String[]{"a", "b", "c"};
        String[] s2 = new String[]{"d", "e", "f"};

        String[] result = ArrayUtils.addAll(s1, s2);

        System.out.println(Arrays.toString(result));

		int [] int1 = new int[]{1,2,3};
        int[] int2 = new int[]{4,5,6};

        int[] result2 = ArrayUtils.addAll(int1, int2);

        System.out.println(Arrays.toString(result2));

    }

} 
```

输出

```java
[a, b, c, d, e, f]
[1, 2, 3, 4, 5, 6]

```

对于 Maven 用户。

pom.xml

```java
 <dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.4</version>
</dependency> 
```

## 2.Java API

纯 Java API 示例，支持原语和泛型类型。

JoinArray.java

```java
 package com.mkyong.example.array;

import java.lang.reflect.Array;
import java.util.Arrays;

public class JoinArray {

    public static void main(String[] args) {

        String[] s1 = new String[]{"a", "b", "c"};
        String[] s2 = new String[]{"d", "e", "f"};
        String[] s3 = new String[]{"g", "h", "i"};

        String[] result = joinArrayGeneric(s1, s2, s3);

        System.out.println(Arrays.toString(result));

        int[] int1 = new int[]{1, 2, 3};
        int[] int2 = new int[]{4, 5, 6};
        int[] int3 = new int[]{7, 8, 9};

        int[] result2 = joinArray(int1, int2, int3);

        System.out.println(Arrays.toString(result2));

    }

    static <T> T[] joinArrayGeneric(T[]... arrays) {
        int length = 0;
        for (T[] array : arrays) {
            length += array.length;
        }

        //T[] result = new T[length];
        final T[] result = (T[]) Array.newInstance(arrays[0].getClass().getComponentType(), length);

        int offset = 0;
        for (T[] array : arrays) {
            System.arraycopy(array, 0, result, offset, array.length);
            offset += array.length;
        }

        return result;
    }

    static int[] joinArray(int[]... arrays) {
        int length = 0;
        for (int[] array : arrays) {
            length += array.length;
        }

        final int[] result = new int[length];

        int offset = 0;
        for (int[] array : arrays) {
            System.arraycopy(array, 0, result, offset, array.length);
            offset += array.length;
        }

        return result;
    }

	//create other overloaded primitive type - long, double...
	//static long[] joinArray(long[]... arrays) 
} 
```

输出

```java
[a, b, c, d, e, f, g, h, i]
[1, 2, 3, 4, 5, 6, 7, 8, 9]

```

## 3.Java 8 流

Java 8 流连接数组的例子。

JoinArray.java

```java
 package com.mkyong.example.array;

import java.util.Arrays;

import java.util.stream.IntStream;
import java.util.stream.Stream;

public class JoinArray {

    public static void main(String[] args) {

        String[] s1 = new String[]{"a", "b", "c"};
        String[] s2 = new String[]{"d", "e", "f"};
        String[] s3 = new String[]{"g", "h", "i"};

		//join object type array
        String[] result = Stream.of(s1, s2, s3).flatMap(Stream::of).toArray(String[]::new);
        System.out.println(Arrays.toString(result));

        int [] int1 = new int[]{1,2,3};
        int[] int2 = new int[]{4,5,6};
        int[] int3 = new int[]{7,8,9};

		//join 2 primitive type array
        int[] result2 = IntStream.concat(Arrays.stream(int1), Arrays.stream(int2)).toArray();

		//join 3 primitive type array, any better idea?
        int[] result3 = IntStream.concat(Arrays.stream(int1), 
			IntStream.concat(Arrays.stream(int2), Arrays.stream(int3))).toArray();

        System.out.println(Arrays.toString(result2));

        System.out.println(Arrays.toString(result3));

    }

} 
```

输出

```java
[a, b, c, d, e, f, g, h, i]
[1, 2, 3, 4, 5, 6]
[1, 2, 3, 4, 5, 6, 7, 8, 9]

```

有更好的 Java 8 流例子吗？请在下面评论。

## 参考

1.  [Apache Commons Lang–ArrayUtils](http://web.archive.org/web/20210819013002/https://commons.apache.org/proper/commons-lang/javadocs/api-3.1/org/apache/commons/lang3/ArrayUtils.html#addAll%28T%5B%5D,%20T...%29)
2.  [Java 8 Stream JavaDoc](http://web.archive.org/web/20210819013002/https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
3.  [Java 8:用 IntStreams 代替传统的 for 循环](http://web.archive.org/web/20210819013002/http://www.deadcoderising.com/2015-05-19-java-8-replace-traditional-for-loops-with-intstreams/)
4.  [Java 8 流教程](http://web.archive.org/web/20210819013002/http://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/)

Tags : [array](http://web.archive.org/web/20210819013002/https://mkyong.com/tag/array/) [java](http://web.archive.org/web/20210819013002/https://mkyong.com/tag/java/) [java8](http://web.archive.org/web/20210819013002/https://mkyong.com/tag/java8/) [stream](http://web.archive.org/web/20210819013002/https://mkyong.com/tag/stream/)<input type="hidden" id="mkyong-current-postId" value="13947">