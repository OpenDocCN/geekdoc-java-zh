# Java 8 Stream–peek()不支持 count()？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-stream-the-peek-is-not-working-with-count/>

许多例子都使用`.count()`作为`.peek()`的终端操作，例如:

Java 8

```java
 List<String> l = Arrays.asList("A", "B", "C", "D");

	long count = l.stream().peek(System.out::println).count();

	System.out.println(count); // 4 
```

输出——工作正常。

```java
 A
B
C
D
4 
```

然而，对于 Java 9 和更高版本，`peek()`可能什么也不打印:

Java 9 and above

```java
 List<String> l = Arrays.asList("A", "B", "C", "D");

	long count = l.stream().peek(System.out::println).count();

	System.out.println(count); // 4 
```

输出

```java
 4 
```

## 为什么 peek()现在什么都不打印？

参考 Java 9 [。count()](http://web.archive.org/web/20220426174442/https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html#count--) Java 文档

```java
 An implementation may choose to not execute the stream pipeline (either sequentially or in parallel) 
if it is capable of computing the count directly from the stream source. 
In such cases no source elements will be traversed and no intermediate operations will be evaluated. 
```

从 Java 9 开始，如果 JDK 编译器能够直接从流中计算出`count`(Java 9 中的优化)，它就不会遍历流，所以根本不需要运行`peek()`。

```java
 List<String> l = Arrays.asList("A", "B", "C", "D");

	// JDK compiler know the size of the stream via the variable l 
	long count = l.stream().peek(System.out::println).count(); 
```

要强制`peek()`运行，只需用`filter()`改变一些元素或切换到另一个终端操作，如`collect()`

filter()

```java
 List<String> l = Arrays.asList("A", "B", "C", "D");

	long count = l.stream()
			.filter(x->!x.isEmpty())
			.peek(System.out::println)
			.count();

	System.out.println(count); // 4 
```

输出

```java
 A
B
C
D
4 
```

collect()

```java
 List<String> l = Arrays.asList("A", "B", "C", "D");

	List<String> result = l.stream()
			.peek(System.out::println)
			.collect(Collectors.toList());

	System.out.println(result.size()); // 4 
```

输出

```java
 A
B
C
D
4 
```

小心混淆`.peek()`和`.count()`，`peek()`在 Java 9 和更高版本中可能无法正常工作。

*P.S .用 Java 12 测试*

# 参考

*   [Stream.count() JavaDocs](http://web.archive.org/web/20220426174442/https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html#count--)
*   [Stream.peek() JavaDocs](http://web.archive.org/web/20220426174442/https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html#peek-java.util.function.Consumer-)

<input type="hidden" id="mkyong-current-postId" value="15126">