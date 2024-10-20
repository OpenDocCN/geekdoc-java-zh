# Java 8 平面图示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-flatmap-example/>

本文解释了 Java 8 [Stream.flatMap](http://web.archive.org/web/20220202084822/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#flatMap-java.util.function.Function-) 以及如何使用它。

主题

1.  [什么是平面图？](#what-is-flatmap)
2.  [为什么要平一条小溪？](#why-flat-a-stream)
3.  [flatMap 示例–查找一套图书。](#flatmap-example---find-all-books)
4.  [平面图示例–订单和行项目。](#flatmap-example---order-and-lineitems)
5.  [平面图示例–按空格分割线条。](#flatmap-example---splits-the-line-by-spaces)
6.  [平面图和原始类型](flatmap-and-primitive-type)

## 1。什么是 flatMap()？

1.1 回顾以下结构。它由 2 级流或 2d 阵列组成。

```java
 # Stream<String[]>
# Stream<Stream<String>>
# String[][]

[
  [1, 2],
  [3, 4],
  [5, 6]
] 
```

在 Java 8 中，我们可以使用`flatMap`将上述两个级别的流转换为一个流级别，或将 2d 数组转换为 1d 数组。

```java
 # Stream<String>
# String[]

[1, 2, 3, 4, 5, 6] 
```

## 2。为什么要平溪？

2.1 处理包含一个以上级别的流很有挑战性，例如`Stream<String[]>`或`Stream<List<LineItem>>`或`Stream<Stream<String>>`。我们将两个级别的流合并成一个级别，像`Stream<String>`或`Stream<LineItem>`，这样我们就可以轻松地循环流并处理它。

查看以下示例，在流上应用`flatMap`之前和之后。

2.2 下面是一个 2d 数组，我们可以用`Arrays.stream`或者`Stream.of`将其转换成一个流，它产生一个`String[]`或者`Stream<String[]>`的流。

```java
 String[][] array = new String[][]{{"a", "b"}, {"c", "d"}, {"e", "f"}};

  // array to a stream
  Stream<String[]> stream1 = Arrays.stream(array);

  // same result
  Stream<String[]> stream2 = Stream.of(array); 
```

或者像这样。

```java
 [
  [a, b],
  [c, d],
  [e, f]
] 
```

2.3 这里是需求，我们要过滤掉`a`，打印出所有的字符。

首先，我们直接试试`Stream#filter`。但是，下面的程序什么都不会打印出来，这是因为`Stream#filter`里面的`x`是一个`String[]`，而不是一个`String`；条件将始终保持为假，流将什么也不收集。

```java
 String[][] array = new String[][]{{"a", "b"}, {"c", "d"}, {"e", "f"}};

  // convert array to a stream
  Stream<String[]> stream1 = Arrays.stream(array);

  List<String[]> result = stream1
      .filter(x -> !x.equals("a"))      // x is a String[], not String!
      .collect(Collectors.toList());

  System.out.println(result.size());    // 0

  result.forEach(System.out::println);  // print nothing? 
```

好了，这一次，我们重构 filter 方法来处理`String[]`。

```java
 String[][] array = new String[][]{{"a", "b"}, {"c", "d"}, {"e", "f"}};

  // array to a stream
  Stream<String[]> stream1 = Arrays.stream(array);

  // x is a String[]
  List<String[]> result = stream1
          .filter(x -> {
              for(String s : x){      // really?
                  if(s.equals("a")){
                      return false;
                  }
              }
              return true;
          }).collect(Collectors.toList());

  // print array
  result.forEach(x -> System.out.println(Arrays.toString(x))); 
```

输出

Terminal

```java
 [c, d]
[e, f] 
```

在上面的例子中，`Stream#filter`将过滤掉整个`[a, b]`，但是我们只想过滤掉字符`a`

下面的 3.4 是最终版本，我们先组合数组，然后是过滤器。在 Java 中，要把一个 2d 数组转换成 1d 数组，我们可以循环 2d 数组，把所有的元素放到一个新数组中；或者我们可以用 Java 8 `flatMap`把 2d 数组扁平化成 1d 数组，或者从`Stream<String[]>`到`Stream<String>`。

```java
 String[][] array = new String[][]{{"a", "b"}, {"c", "d"}, {"e", "f"}};

  // Java 8
  String[] result = Stream.of(array)  // Stream<String[]>
          .flatMap(Stream::of)        // Stream<String>
          .toArray(String[]::new);    // [a, b, c, d, e, f]

  for (String s : result) {
      System.out.println(s);
  } 
```

输出

Terminal

```java
 a
b
c
d
e
f 
```

现在，我们可以很容易地过滤掉`a`；让我们看看最终版本。

```java
 String[][] array = new String[][]{{"a", "b"}, {"c", "d"}, {"e", "f"}};

  List<String> collect = Stream.of(array)     // Stream<String[]>
          .flatMap(Stream::of)                // Stream<String>
          .filter(x -> !"a".equals(x))        // filter out the a
          .collect(Collectors.toList());      // return a List

  collect.forEach(System.out::println); 
```

输出

Terminal

```java
 b
c
d
e
f 
```

我想指出的是，处理多于一个级别的流是具有挑战性的，令人困惑，并且容易出错，我们可以使用这个`Stream#flatMap`将 2 个级别的流展平为一个级别的流。

```java
 Stream<String[]>      -> flatMap ->	Stream<String>
Stream<Set<String>>   -> flatMap ->	Stream<String>
Stream<List<String>>  -> flatMap ->	Stream<String>
Stream<List<Object>>  -> flatMap ->	Stream<Object> 
```

## 3。平面图示例-查找所有书籍。

这个例子使用`.stream()`将一个`List`转换成一个对象流，每个对象包含一套书，我们可以使用`flatMap`产生一个包含所有对象中所有书的流。

最后，我们还过滤掉包含单词`python`的书，并收集一个`Set`来移除重复的书。

Developer.java

```java
 package com.mkyong.java8.stream.flatmap;

import java.util.HashSet;
import java.util.Set;

public class Developer {

    private Integer id;
    private String name;
    private Set<String> book;

    //getters, setters, toString

    public void addBook(String book) {
        if (this.book == null) {
            this.book = new HashSet<>();
        }
        this.book.add(book);
    }
} 
```

FlatMapExample1.java

```java
 package com.mkyong.java8.stream.flatmap;

import java.util.ArrayList;
import java.util.List;
import java.util.Set;
import java.util.stream.Collectors;

public class FlatMapExample1 {

    public static void main(String[] args) {

        Developer o1 = new Developer();
        o1.setName("mkyong");
        o1.addBook("Java 8 in Action");
        o1.addBook("Spring Boot in Action");
        o1.addBook("Effective Java (3nd Edition)");

        Developer o2 = new Developer();
        o2.setName("zilap");
        o2.addBook("Learning Python, 5th Edition");
        o2.addBook("Effective Java (3nd Edition)");

        List<Developer> list = new ArrayList<>();
        list.add(o1);
        list.add(o2);

        // hmm....Set of Set...how to process?
        /*Set<Set<String>> collect = list.stream()
                .map(x -> x.getBook())
                .collect(Collectors.toSet());*/

        Set<String> collect =
                list.stream()
                        .map(x -> x.getBook())                              //  Stream<Set<String>>
                        .flatMap(x -> x.stream())                           //  Stream<String>
                        .filter(x -> !x.toLowerCase().contains("python"))   //  filter python book
                        .collect(Collectors.toSet());                       //  remove duplicated

        collect.forEach(System.out::println);

    }

} 
```

输出

Terminal

```java
 Spring Boot in Action
Effective Java (3nd Edition)
Java 8 in Action 
```

`map`是可选的。

```java
 Set<String> collect2 = list.stream()
                    //.map(x -> x.getBook())
                    .flatMap(x -> x.getBook().stream())                 //  Stream<String>
                    .filter(x -> !x.toLowerCase().contains("python"))   //  filter python book
                    .collect(Collectors.toSet()); 
```

## 4。平面图示例–订单和行项目

这个例子类似于官方的 [flatMap JavaDoc](http://web.archive.org/web/20220202084822/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#flatMap-java.util.function.Function-) 例子。

`orders`是一个购买订单流，每个购买订单包含一组行项目，然后我们可以使用`flatMap`产生一个包含所有订单中所有行项目的流或`Stream<LineItem>`。此外，我们还添加了一个`reduce`操作来合计行项目的总金额。

FlatMapExample2.java

```java
 package com.mkyong.java8.stream.flatmap;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;

public class FlatMapExample2 {

    public static void main(String[] args) {

        List<Order> orders = findAll();

        /*
            Stream<List<LineItem>> listStream = orders.stream()
                    .map(order -> order.getLineItems());

            Stream<LineItem> lineItemStream = orders.stream()
                    .flatMap(order -> order.getLineItems().stream());
        */

        // sum the line items' total amount
        BigDecimal sumOfLineItems = orders.stream()
                .flatMap(order -> order.getLineItems().stream())    //  Stream<LineItem>
                .map(line -> line.getTotal())                       //  Stream<BigDecimal>
                .reduce(BigDecimal.ZERO, BigDecimal::add);          //  reduce to sum all

        // sum the order's total amount
        BigDecimal sumOfOrder = orders.stream()
                .map(order -> order.getTotal())                     //  Stream<BigDecimal>
                .reduce(BigDecimal.ZERO, BigDecimal::add);          //  reduce to sum all

        System.out.println(sumOfLineItems);                         // 3194.20
        System.out.println(sumOfOrder);                             // 3194.20

        if (!sumOfOrder.equals(sumOfLineItems)) {
            System.out.println("The sumOfOrder is not equals to sumOfLineItems!");
        }

    }

    // create dummy records
    private static List<Order> findAll() {

        LineItem item1 = new LineItem(1, "apple", 1, new BigDecimal("1.20"), new BigDecimal("1.20"));
        LineItem item2 = new LineItem(2, "orange", 2, new BigDecimal(".50"), new BigDecimal("1.00"));
        Order order1 = new Order(1, "A0000001", Arrays.asList(item1, item2), new BigDecimal("2.20"));

        LineItem item3 = new LineItem(3, "monitor BenQ", 5, new BigDecimal("99.00"), new BigDecimal("495.00"));
        LineItem item4 = new LineItem(4, "monitor LG", 10, new BigDecimal("120.00"), new BigDecimal("1200.00"));
        Order order2 = new Order(2, "A0000002", Arrays.asList(item3, item4), new BigDecimal("1695.00"));

        LineItem item5 = new LineItem(5, "One Plus 8T", 3, new BigDecimal("499.00"), new BigDecimal("1497.00"));
        Order order3 = new Order(3, "A0000003", Arrays.asList(item5), new BigDecimal("1497.00"));

        return Arrays.asList(order1, order2, order3);

    }
} 
```

Order.java

```java
 public class Order {

    private Integer id;
    private String invoice;
    private List<LineItem> lineItems;
    private BigDecimal total;

    // getter, setters, constructor
} 
```

LineItem.java

```java
 public class LineItem {

    private Integer id;
    private String item;
    private Integer qty;
    private BigDecimal price;
    private BigDecimal total;

    // getter, setters, constructor
} 
```

输出

Terminal

```java
 3194.20
3194.20 
```

## 5。平面图示例–用空格分割线条

这个例子读取了一个文本文件，用空格分割了一行，并显示了单词的总数。

一个文本文件。

c:\\test\\test.txt

```java
 hello world Java
hello world Python
hello world Node JS
hello world Rust
hello world Flutter 
```

阅读评论，不言自明。

FlatMapExample3.java

```java
 package com.mkyong.java8.stream.flatmap;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.stream.Stream;

public class FlatMapExample3 {

    public static void main(String[] args) throws IOException {

        Path path = Paths.get("C:\\test\\test.txt");

        // read file into a stream of lines
        Stream<String> lines = Files.lines(path, StandardCharsets.UTF_8);

        // stream of array...hard to process.
        // Stream<String[]> words = lines.map(line -> line.split(" +"));

        // stream of stream of string....hmm...better flat to one level.
        // Stream<Stream<String>> words = lines.map(line -> Stream.of(line.split(" +")));

        // result a stream of words, good!
        Stream<String> words = lines.flatMap(line -> Stream.of(line.split(" +")));

        // count the number of words.
        long noOfWords = words.count();

        System.out.println(noOfWords);  // 16
    }
} 
```

输出

Terminal

```java
 16 
```

## 6。平面图和原始类型

对于原始类型，如`int`、`long`、`double`等。Java 8 流也提供相关的`flatMapTo{primative type}`来扁平化原语类型的流；概念是一样的。

`flatMapToInt`->-`IntStream`

FlatMapExample4.java

```java
 package com.mkyong.java8.stream.flatmap;

import java.util.Arrays;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class FlatMapExample4 {

    public static void main(String[] args) {

        int[] array = {1, 2, 3, 4, 5, 6};

        //Stream<int[]>
        Stream<int[]> streamArray = Stream.of(array);

        //Stream<int[]> -> flatMap -> IntStream
        IntStream intStream = streamArray.flatMapToInt(x -> Arrays.stream(x));

        intStream.forEach(x -> System.out.println(x));

    }
} 
```

输出

Terminal

```java
 1
2
3
4
5
6 
```

`flatMapToLong`->-`LongStream`

```java
 long[] array = {1, 2, 3, 4, 5, 6};

  Stream<long[]> longArray = Stream.of(array);

  LongStream longStream = longArray.flatMapToLong(x -> Arrays.stream(x));

  System.out.println(longStream.count()); 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220202084822/https://github.com/mkyong/core-java)

$ cd java8

## 参考文献

*   [Stream.flatMap JavaDoc](http://web.archive.org/web/20220202084822/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#flatMap-java.util.function.Function-)
*   [地图()平面地图()之间的差异](http://web.archive.org/web/20220202084822/https://stackoverflow.com/questions/26684562/whats-the-difference-between-map-and-flatmap-methods-in-java-8)
*   [Java–如何连接数组](/web/20220202084822/https://mkyong.com/java/java-how-to-join-arrays/)
*   [Java 8–如何打印数组](/web/20220202084822/https://mkyong.com/java/java-how-to-print-an-array/)
*   [Java 8–收集器分组和映射示例](/web/20220202084822/https://mkyong.com/java8/java-8-collectors-groupingby-and-mapping-example/)
*   [Java 8 Stream.reduce()示例](/web/20220202084822/https://mkyong.com/java8/java-8-stream-reduce-examples/)

<input type="hidden" id="mkyong-current-postId" value="13973">