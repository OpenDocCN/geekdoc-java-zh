# Java——如何对所有流整数求和

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-how-to-sum-all-the-stream-integers/>

`sum()`方法在原始 int-value 流中可用，如`IntStream`，而不是`Stream<Integer>`。我们可以使用`mapToInt()`将一个流整数转换成一个`IntStream`。

```java
 int sum = integers.stream().mapToInt(Integer::intValue).sum();
int sum = integers.stream().mapToInt(x -> x).sum(); 
```

完整的例子。

Java8Stream.java

```java
 package com.mkyong.concurrency;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class Java8Stream {

    public static void main(String[] args) {

        List<Integer> integers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        int sum = integers.stream().mapToInt(Integer::intValue).sum();
        System.out.println("Total : " + sum);

        Stream<Integer> integers2 = Stream.iterate(1, n -> n + 1).limit(10);
        IntStream intStream = integers2.mapToInt(x -> x);
        int sum1 = intStream.sum();
        System.out.println("Total : " + sum1);

    }

} 
```

输出

```java
 Total : 55
Total : 55 
```

## 参考

1.  [甲骨文文件–IntStream.html](http://web.archive.org/web/20210817092447/https://docs.oracle.com/javase/8/docs/api/java/util/stream/IntStream.html)

Tags : [java 8](http://web.archive.org/web/20210817092447/https://mkyong.com/tag/java-8/) [sum](http://web.archive.org/web/20210817092447/https://mkyong.com/tag/sum/)<input type="hidden" id="mkyong-current-postId" value="14838">