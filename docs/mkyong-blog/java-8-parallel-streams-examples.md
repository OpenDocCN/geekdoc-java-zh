# Java 8 并行流示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-parallel-streams-examples/>

![parallel streams high load](img/f153d608228c454003c61fe5c4fda20f.png)

几个并行执行流的 Java 8 例子。

## 1.BaseStream.parallel()

打印 1 到 10 的简单并行示例。

ParallelExample1.java

```java
 package com.mkyong.java8;

import java.util.stream.IntStream;

public class ParallelExample1 {

    public static void main(String[] args) {

        System.out.println("Normal...");

		IntStream range = IntStream.rangeClosed(1, 10);
        range.forEach(System.out::println);

        System.out.println("Parallel...");

        IntStream range2 = IntStream.rangeClosed(1, 10);
        range2.parallel().forEach(System.out::println);

    }

} 
```

输出

```java
 Normal...
1
2
3
4
5
6
7
8
9
10

Parallel...
7
6
8
9
10
1
4
5
3
2 
```

## 2.Collection.parallelStream()

另一个简单的并行例子是打印`a`到`z`。对于收藏，我们可以用`parallelStream()`。

ParallelExample2.java

```java
 package com.mkyong.java8;

import java.util.ArrayList;
import java.util.List;

public class ParallelExample2 {

    public static void main(String[] args) {

        System.out.println("Normal...");

        List<String> alpha = getData();
        alpha.stream().forEach(System.out::println);

        System.out.println("Parallel...");

        List<String> alpha2 = getData();
        alpha2.parallelStream().forEach(System.out::println);

    }

    private static List<String> getData() {

        List<String> alpha = new ArrayList<>();

        int n = 97;  // 97 = a , 122 = z
        while (n <= 122) {
            char c = (char) n;
            alpha.add(String.valueOf(c));
            n++;
        }

        return alpha;

    }

} 
```

输出

```java
 Normal...
a
b
c
d
e
f
g
h
i
j
k
l
m
n
o
p
q
r
s
t
u
v
w
x
y
z
Parallel...
q
s
r
o
x
h
l
p
d
i
g
t
u
n
z
v
j
k
w
f
m
c
a
e
b
y 
```

## 3.Stream 是否以并行模式运行？

3.1 我们可以用`isParallel()`来测试

ParallelExample3a.java

```java
 package com.mkyong.java8;

import java.util.stream.IntStream;

public class ParallelExample3a {

    public static void main(String[] args) {

        System.out.println("Normal...");

        IntStream range = IntStream.rangeClosed(1, 10);
        System.out.println(range.isParallel());         // false
        range.forEach(System.out::println);

        System.out.println("Parallel...");

        IntStream range2 = IntStream.rangeClosed(1, 10);
        IntStream range2Parallel = range2.parallel();
        System.out.println(range2Parallel.isParallel()); // true
        range2Parallel.forEach(System.out::println);

    }

} 
```

3.2 或者像这样打印当前线程名:

ParallelExample3b.java

```java
 package com.mkyong.java8;

import java.util.stream.IntStream;

public class ParallelExample3b {

    public static void main(String[] args) {

        System.out.println("Normal...");

        IntStream range = IntStream.rangeClosed(1, 10);
        range.forEach(x -> {
            System.out.println("Thread : " + Thread.currentThread().getName() + ", value: " + x);
        });

        System.out.println("Parallel...");

        IntStream range2 = IntStream.rangeClosed(1, 10);
        range2.parallel().forEach(x -> {
            System.out.println("Thread : " + Thread.currentThread().getName() + ", value: " + x);
        });

    }

} 
```

输出

```java
 Normal...
Thread : main, value: 1
Thread : main, value: 2
Thread : main, value: 3
Thread : main, value: 4
Thread : main, value: 5
Thread : main, value: 6
Thread : main, value: 7
Thread : main, value: 8
Thread : main, value: 9
Thread : main, value: 10

Parallel...
Thread : main, value: 7
Thread : main, value: 6
Thread : ForkJoinPool.commonPool-worker-5, value: 3
Thread : ForkJoinPool.commonPool-worker-7, value: 8
Thread : ForkJoinPool.commonPool-worker-5, value: 5
Thread : ForkJoinPool.commonPool-worker-5, value: 4
Thread : ForkJoinPool.commonPool-worker-3, value: 9
Thread : ForkJoinPool.commonPool-worker-5, value: 1
Thread : ForkJoinPool.commonPool-worker-7, value: 2
Thread : ForkJoinPool.commonPool-worker-9, value: 10 
```

默认情况下，并行流使用“forkjoinpool”

## 4.计算

4.1 Java 8 流打印所有 100 万以内的质数:

ParallelExample4.java

```java
 package com.mkyong.java8;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class ParallelExample4 {

    public static void main(String[] args) {

        long count = Stream.iterate(0, n -> n + 1)
                .limit(1_000_000)
                //.parallel()   with this 23s, without this 1m 10s
                .filter(ParallelExample4::isPrime)
                .peek(x -> System.out.format("%s\t", x))
                .count();

        System.out.println("\nTotal: " + count);

    }

    public static boolean isPrime(int number) {
        if (number <= 1) return false;
        return !IntStream.rangeClosed(2, number / 2).anyMatch(i -> number % i == 0);
    }

} 
```

结果:

*   对于普通流，需要 1 分 10 秒。
*   对于并行流，需要 23 秒。

*PS 测试使用 i7-7700，16G 内存，WIndows 10*

4.2 另一个并行流示例，用于找出雇员列表的平均年龄。

```java
 List<Employee> employees = obj.generateEmployee(10000);

	double age = employees
			.parallelStream()
			.mapToInt(Employee::getAge)
			.average()
			.getAsDouble();

    System.out.println("Average age: " + age); 
```

## 5.个案研究

5.1 并行流提高了耗时的保存文件任务的性能。

这段 Java 代码将生成 10，000 个随机雇员，并保存到 10，000 个文件中，每个雇员保存到一个文件中。

*   正常流的话需要 27-29 秒。
*   对于并行流，需要 7-8 秒。

*PS 测试使用 i7-7700，16G 内存，WIndows 10*

ParallelExample5.java

```java
 package com.mkyong.java8;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;
import java.math.BigDecimal;
import java.math.RoundingMode;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;
import java.util.Random;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class ParallelExample5 {

    private static final String DIR = System.getProperty("user.dir") + "/test/";

    public static void main(String[] args) throws IOException {

        Files.createDirectories(Paths.get(DIR));

        ParallelExample5 obj = new ParallelExample5();

        List<Employee> employees = obj.generateEmployee(10000);

		// normal, sequential
        //employees.stream().forEach(ParallelExample5::save); 		// 27s-29s

		// parallel
        employees.parallelStream().forEach(ParallelExample5::save); // 7s-8s

    }

    private static void save(Employee input) {

        try (FileOutputStream fos = new FileOutputStream(new File(DIR + input.getName() + ".txt"));
             ObjectOutputStream obs = new ObjectOutputStream(fos)) {
            obs.writeObject(input);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    private List<Employee> generateEmployee(int num) {

        return Stream.iterate(0, n -> n + 1)
                .limit(num)
                .map(x -> {
                    return new Employee(
                            generateRandomName(4),
                            generateRandomAge(15, 100),
                            generateRandomSalary(900.00, 200_000.00)
                    );
                })
                .collect(Collectors.toList());

    }

    private String generateRandomName(int length) {

        return new Random()
                .ints(5, 97, 122) // 97 = a , 122 = z
                .mapToObj(x -> String.valueOf((char) x))
                .collect(Collectors.joining());

    }

    private int generateRandomAge(int min, int max) {
        return new Random()
                .ints(1, min, max)
                .findFirst()
                .getAsInt();
    }

    private BigDecimal generateRandomSalary(double min, double max) {
        return new BigDecimal(new Random()
                .doubles(1, min, max)
                .findFirst()
                .getAsDouble()).setScale(2, RoundingMode.HALF_UP);
    }

} 
```

Employee.java

```java
 package com.mkyong.java8;

import java.io.Serializable;
import java.math.BigDecimal;

public class Employee implements Serializable {

    private static final long serialVersionUID = 1L;

    private String name;
    private int age;
    private BigDecimal salary;

    //getters, setters n etc...

} 
```

# 参考

*   [Java 教程–并行性](http://web.archive.org/web/20220204092017/https://docs.oracle.com/javase/tutorial/collections/streams/parallelism.html)
*   [Java 8 Stream.iterate 示例](http://web.archive.org/web/20220204092017/https://www.mkyong.com/java8/java-8-stream-iterate-examples/)
*   [Java 质数示例](http://web.archive.org/web/20220204092017/https://www.mkyong.com/java/java-prime-numbers-examples/)
*   [Java 8 中的错误，第三部分:流和并行流](http://web.archive.org/web/20220204092017/https://dzone.com/articles/whats-wrong-java-8-part-iii)

<input type="hidden" id="mkyong-current-postId" value="15136">