# Java——生成一个范围内的随机整数

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-generate-random-integers-in-a-range/>

![java-random-integer-in-range](img/48800849bd5f64464ad0706241d1a467.png)

在本文中，我们将向您展示在一个范围内生成随机整数的三种方法。

1.  java.util.Random.nextInt
2.  数学.随机
3.  java.util.Random.ints (Java 8)

## 1.java.util.Random

此`Random().nextInt(int bound)`生成一个从 0(含)到界(不含)的随机整数。

1.1 代码片段。对于`getRandomNumberInRange(5, 10)`，这将生成一个介于 5(含)和 10(含)之间的随机整数。

```java
 private static int getRandomNumberInRange(int min, int max) {

		if (min >= max) {
			throw new IllegalArgumentException("max must be greater than min");
		}

		Random r = new Random();
		return r.nextInt((max - min) + 1) + min;
	} 
```

1.2 什么是(最大–最小)+ 1) +最小？

上述公式将生成一个介于最小值(含)和最大值(含)之间的随机整数。

```java
 //Random().nextInt(int bound) = Random integer from 0 (inclusive) to bound (exclusive)

	//1\. nextInt(range) = nextInt(max - min)
	new Random().nextInt(5);  // [0...4] [min = 0, max = 4]
	new Random().nextInt(6);  // [0...5]
	new Random().nextInt(7);  // [0...6]
	new Random().nextInt(8);  // [0...7]
	new Random().nextInt(9);  // [0...8]
	new Random().nextInt(10); // [0...9]			
	new Random().nextInt(11); // [0...10]

	//2\. To include the last value (max value) = (range + 1)
	new Random().nextInt(5 + 1)  // [0...5] [min = 0, max = 5]
	new Random().nextInt(6 + 1)  // [0...6]
	new Random().nextInt(7 + 1)  // [0...7]
	new Random().nextInt(8 + 1)  // [0...8]
	new Random().nextInt(9 + 1)  // [0...9]
	new Random().nextInt(10 + 1) // [0...10]			
	new Random().nextInt(11 + 1) // [0...11]

	//3\. To define a start value (min value) in a range,
	//   For example, the range should start from 10 = (range + 1) + min
	new Random().nextInt(5 + 1)  + 10 // [0...5]  + 10 = [10...15]
	new Random().nextInt(6 + 1)  + 10 // [0...6]  + 10 = [10...16]
	new Random().nextInt(7 + 1)  + 10 // [0...7]  + 10 = [10...17]
	new Random().nextInt(8 + 1)  + 10 // [0...8]  + 10 = [10...18]
	new Random().nextInt(9 + 1)  + 10 // [0...9]  + 10 = [10...19]
	new Random().nextInt(10 + 1) + 10 // [0...10] + 10 = [10...20]
	new Random().nextInt(11 + 1) + 10 // [0...11] + 10 = [10...21]

	// Range = (max - min)
	// So, the final formula is ((max - min) + 1) + min

	//4\. Test [10...30]
	// min = 10 , max = 30, range = (max - min)
	new Random().nextInt((max - min) + 1) + min
	new Random().nextInt((30 - 10) + 1) + 10
	new Random().nextInt((20) + 1) + 10
	new Random().nextInt(21) + 10    //[0...20] + 10 = [10...30]

	//5\. Test [15...99]
	// min = 15 , max = 99, range = (max - min)
	new Random().nextInt((max - min) + 1) + min
	new Random().nextInt((99 - 15) + 1) + 15
	new Random().nextInt((84) + 1) + 15
	new Random().nextInt(85) + 15    //[0...84] + 15 = [15...99]

	//Done, understand? 
```

1.3 产生 10 个范围在 5(含)和 10(含)之间的随机整数的完整示例。

TestRandom.java

```java
 package com.mkyong.example.test;

import java.util.Random;

public class TestRandom {

	public static void main(String[] args) {

		for (int i = 0; i < 10; i++) {
			System.out.println(getRandomNumberInRange(5, 10));
		}

	}

	private static int getRandomNumberInRange(int min, int max) {

		if (min >= max) {
			throw new IllegalArgumentException("max must be greater than min");
		}

		Random r = new Random();
		return r.nextInt((max - min) + 1) + min;
	}

} 
```

输出。

```java
 7
6
10
8
9
5
7
10
8
5 
```

## 2.数学.随机

这个`Math.random()`给出一个从 0.0(含)到 1.0(不含)的随机双精度。

2.1 代码片段。参考 1.2，或多或少是同一个公式。

```java
 (int)(Math.random() * ((max - min) + 1)) + min 
```

2.2 生成 10 个随机整数的完整示例，范围在 16(含)和 20(含)之间。

TestRandom.java

```java
 package com.mkyong.example.test;

public class TestRandom {

	public static void main(String[] args) {

		for (int i = 0; i < 10; i++) {
			System.out.println(getRandomNumberInRange(16, 20));
		}

	}

	private static int getRandomNumberInRange(int min, int max) {

		if (min >= max) {
			throw new IllegalArgumentException("max must be greater than min");
		}

		return (int)(Math.random() * ((max - min) + 1)) + min;
	}

} 
```

输出。

```java
 17
16
20
19
20
20
20
17
20
16 
```

**Note**
The `Random.nextInt(n)` is more efficient than `Math.random() * n`, read this [Oracle forum post](http://web.archive.org/web/20220725120920/https://community.oracle.com/message/6596485#thread-message-6596485).

## 3.Java 8 Random.ints

在 Java 8 中，在`java.util.Random`中增加了新的方法

```java
 public IntStream ints(int randomNumberOrigin, int randomNumberBound)
	public IntStream ints(long streamSize, int randomNumberOrigin, int randomNumberBound) 
```

这个`Random.ints(int origin, int bound)`或`Random.ints(int min, int max)`生成一个从原点(含)到界(不含)的随机整数。

3.1 代码片段。

```java
 private static int getRandomNumberInRange(int min, int max) {

		Random r = new Random();
		return r.ints(min, (max + 1)).findFirst().getAsInt();

	} 
```

3.2 产生 10 个范围在 33(含)和 38(含)之间的随机整数的完整示例。

TestRandom.java

```java
 package com.mkyong.form.test;

import java.util.Random;

public class TestRandom {

	public static void main(String[] args) {

		for (int i = 0; i < 10; i++) {
			System.out.println(getRandomNumberInRange(33, 38));
		}

	}

	private static int getRandomNumberInRange(int min, int max) {

		Random r = new Random();
		return r.ints(min, (max + 1)).limit(1).findFirst().getAsInt();

	}

} 
```

输出。

```java
 34
35
37
33
38
37
34
35
36
37 
```

3.3 额外，供自我参考。

生成介于 33(含)和 38(不含)之间的随机整数，流大小为 10。并打印出带有`forEach`的项目。

```java
 //Java 8 only
	new Random().ints(10, 33, 38).forEach(System.out::println); 
```

输出。

```java
 34
37
37
34
34
35
36
33
37
34 
```

## 参考

1.  [java.util.Random JavaDoc](http://web.archive.org/web/20220725120920/https://docs.oracle.com/javase/8/docs/api/java/util/Random.html)
2.  [java.lang.Math JavaDoc](http://web.archive.org/web/20220725120920/https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html)
3.  [甲骨文论坛:随机数生成](http://web.archive.org/web/20220725120920/https://community.oracle.com/message/6596485)
4.  [在 JavaScript 中生成加权随机数](http://web.archive.org/web/20220725120920/http://www.javascriptkit.com/javatutors/weighrandom.shtml)

<input type="hidden" id="mkyong-current-postId" value="13845">