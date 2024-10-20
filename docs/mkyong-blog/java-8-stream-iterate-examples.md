# Java 8 Stream.iterate 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-stream-iterate-examples/>

在 Java 8 中，我们可以使用`Stream.iterate`按需创建流值，也就是所谓的无限流。

## 1.Stream.iterate

1.1 0–9 的流

```java
 //Stream.iterate(initial value, next value)
	Stream.iterate(0, n -> n + 1)
                .limit(10)
                .forEach(x -> System.out.println(x)); 
```

输出

```java
 0
1
2
3
4
5
6
7
8
9 
```

1.2 仅奇数流。

```java
 Stream.iterate(0, n -> n + 1)
			.filter(x -> x % 2 != 0) //odd
			.limit(10)
			.forEach(x -> System.out.println(x)); 
```

输出

```java
 1
3
5
7
9
11
13
15
17
19 
```

1.3 一个经典的斐波那契例子。

```java
 Stream.iterate(new int[]{0, 1}, n -> new int[]{n[1], n[0] + n[1]})
		.limit(20)
		.map(n -> n[0])
		.forEach(x -> System.out.println(x)); 
```

输出

```java
 0
1
1
2
3
5
8
13
21
34
55
89
144
233
377
610
987
1597
2584
4181 
```

1.4 对所有斐波纳契值求和。

```java
 int sum = Stream.iterate(new int[]{0, 1}, n -> new int[]{n[1], n[0] + n[1]})
                .limit(10)
                .map(n -> n[0]) // Stream<Integer>
                .mapToInt(n -> n)
                .sum();

        System.out.println("Fibonacci 10 sum : " + sum); 
```

输出

```java
 Fibonacci 10 sum : 88 
```

## 2.Java 9

Java 9 中增强了`stream.iterate`。它支持一个谓词(条件)作为第二个参数，如果谓词为假，`stream.iterate`将停止。

2.1 如果`n >= 20`，停止流迭代

```java
 Stream.iterate(1, n -> n < 20 , n -> n * 2)
           .forEach(x -> System.out.println(x)); 
```

输出

```java
 1
2
4
8
16 
```

## 参考

1.  [Oracle 文档–Java 流迭代](http://web.archive.org/web/20220426174440/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#iterate-T-java.util.function.UnaryOperator-)

<input type="hidden" id="mkyong-current-postId" value="14837">