# Java 8 forEach 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-foreach-examples/>

在 Java 8 中，我们可以使用新的`forEach`来循环或迭代一个`Map`、`List`、`Set`或`Stream`。

主题

1.  [循环地图](#loop-a-map)
2.  [循环列表](#loop-a-list)
3.  [forEach 和 Consumer](#foreach-and-consumer)
4.  [forEach 和异常处理](#foreach-and-exception-handling)
5.  [forEach vs forEachOrdered](#foreach-vs-foreachordered)

## 1。循环地图

1.1 下面是循环 a `Map`的正常方式。

```java
 public static void loopMapClassic() {

      Map<String, Integer> map = new HashMap<>();
      map.put("A", 10);
      map.put("B", 20);
      map.put("C", 30);
      map.put("D", 40);
      map.put("E", 50);
      map.put("F", 60);

      for (Map.Entry<String, Integer> entry : map.entrySet()) {
          System.out.println("Key : " + entry.getKey() + ", Value : " + entry.getValue());
      }

  } 
```

1.2 在 Java 8 中，我们可以使用`forEach`来循环一个`Map`并打印出它的条目。

```java
 public static void loopMapJava8() {

      Map<String, Integer> map = new HashMap<>();
      map.put("A", 10);
      map.put("B", 20);
      map.put("C", 30);
      map.put("D", 40);
      map.put("E", 50);
      map.put("F", 60);

      // lambda
      map.forEach((k, v) -> System.out.println("Key : " + k + ", Value : " + v));

  } 
```

输出

Terminal

```java
 Key : A, Value : 10
Key : B, Value : 20
Key : C, Value : 30
Key : D, Value : 40
Key : E, Value : 50
Key : F, Value : 60 
```

1.3 对于`Map`包含`null`的键或值，`forEach`将打印`null`。

```java
 public static void loopMapJava8() {

      Map<String, Integer> map = new HashMap<>();
      map.put("A", 10);
      map.put("B", 20);
      map.put("C", 30);
      map.put(null, 40);
      map.put("E", null);
      map.put("F", 60);

      // ensure map is not null
      if (map != null) {
          map.forEach((k, v) -> System.out.println("Key : " + k + ", Value : " + v));
      }

  } 
```

输出

Terminal

```java
 Key : null, Value : 40
Key : A, Value : 10
Key : B, Value : 20
Key : C, Value : 30
Key : E, Value : null
Key : F, Value : 60 
```

*P.S 循环 a `Map`的正常方式将打印与上面相同的输出。*

1.4 如果我们不想打印`null`键，在`forEach`内添加一个简单的空值检查。

```java
 public static void loopMapJava8() {

      Map<String, Integer> map = new HashMap<>();
      map.put("A", 10);
      map.put("B", 20);
      map.put("C", 30);
      map.put(null, 40);
      map.put("E", null);
      map.put("F", 60);

      map.forEach(
          (k, v) -> {
              // yes, we can put logic here
              if (k != null){
                  System.out.println("Key : " + k + ", Value : " + v);
              }
          }
      );

  } 
```

输出

Terminal

```java
 Key : A, Value : 10
Key : B, Value : 20
Key : C, Value : 30
Key : E, Value : null
Key : F, Value : 60 
```

## 2。循环列表

2.1 下面是循环 a `List`的正常方式。

```java
 public static void loopListClassic() {

      List<String> list = new ArrayList<>();
      list.add("A");
      list.add("B");
      list.add("C");
      list.add("D");
      list.add("E");

      // normal loop
      for (String l : list) {
          System.out.println(l);
      }

  } 
```

2.2 Java 8 `forEach`来循环一个`List`。

```java
 public static void loopListJava8() {

      List<String> list = new ArrayList<>();
      list.add("A");
      list.add("B");
      list.add("C");
      list.add("D");
      list.add("E");

      // lambda
      // list.forEach(x -> System.out.println(x));

      // method reference
      list.forEach(System.out::println);
  } 
```

输出

Terminal

```java
 A
B
C
D
E 
```

2.3 这个例子过滤了一个`List`的空值。

```java
 public static void loopListJava8() {

      List<String> list = new ArrayList<>();
      list.add("A");
      list.add("B");
      list.add(null);
      list.add("D");
      list.add("E");

      // filter null value
      list.stream()
              .filter(Objects::nonNull)
              .forEach(System.out::println);

  } 
```

输出

Terminal

```java
 A
B
D
E 
```

*附注:`Set`和`Stream`的`forEach`工作方式相同。*

## 3。forEach 和 Consumer

3.1 检查`forEach`方法签名，它接受一个函数接口[消费者](/web/20220627140650/https://mkyong.com/java8/java-8-consumer-examples/)。

Iterable.java

```java
 public interface Iterable<T> {

  default void forEach(Consumer<? super T> action) {
      Objects.requireNonNull(action);
      for (T t : this) {
          action.accept(t);
      }
  }
  //..
} 
```

Stream.java

```java
 public interface Stream<T> extends BaseStream<T, Stream<T>> {

  void forEach(Consumer<? super T> action);
  //...

} 
```

3.2 本例创建了一个`Consumer`方法，将字符串打印成十六进制格式。我们现在可以重用同一个`Consumer`方法，并将其传递给`List`和`Stream`的`forEach`方法。

ForEachConsumer.java

```java
 package com.mkyong.java8.misc;

import java.util.*;
import java.util.function.Consumer;
import java.util.stream.Stream;

public class ForEachConsumer {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("abc", "java", "python");
        Stream<String> stream = Stream.of("abc", "java", "python");

        // convert a String to a Hex
        Consumer<String> printTextInHexConsumer = (String x) -> {
            StringBuilder sb = new StringBuilder();
            for (char c : x.toCharArray()) {
                String hex = Integer.toHexString(c);
                sb.append(hex);
            }
            System.out.print(String.format("%n%-10s:%s", x, sb.toString()));
        };

        // pass a Consumer
        list.forEach(printTextInHexConsumer);

        stream.forEach(printTextInHexConsumer);

    }

} 
```

输出

Terminal

```java
 abc       :616263
java      :6a617661
python    :707974686f6e

abc       :616263
java      :6a617661
python    :707974686f6e 
```

## 4。forEach 和异常处理。

4.1`forEach`不仅仅是为了打印，这个例子展示了如何使用`forEach`方法循环一个对象列表并将其写入文件。

ForEachWriteFile.java

```java
 package com.mkyong.java8.misc;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Arrays;
import java.util.List;

public class ForEachWriteFile {

    public static void main(String[] args) {

        ForEachWriteFile obj = new ForEachWriteFile();
        obj.save(Paths.get("C:\\test"), obj.createDummyFiles());
    }

    public void save(Path path, List<DummyFile> files) {

        if (!Files.isDirectory(path)) {
            throw new IllegalArgumentException("Path must be a directory");
        }

        files.forEach(f -> {
            try {
                int id = f.getId();
                // create a filename
                String fileName = id + ".txt";
                Files.write(path.resolve(fileName),
                        f.getContent().getBytes(StandardCharsets.UTF_8));
            } catch (IOException e) {
                e.printStackTrace();
            }
        });

    }

    public List<DummyFile> createDummyFiles() {
        return Arrays.asList(
                new DummyFile(1, "hello"),
                new DummyFile(2, "world"),
                new DummyFile(3, "java"));
    }

    class DummyFile {
        int id;
        String content;

        public DummyFile(int id, String content) {
            this.id = id;
            this.content = content;
        }

        public int getId() {
            return id;
        }

        public String getContent() {
            return content;
        }
    }
} 
```

上面的程序将创建三个文本文件。

C:\\test\\1.txt

```java
 hello 
```

C:\\test\\2.txt

```java
 world 
```

C:\\test\\3.txt

```java
 java 
```

4.2`Files.write`可能抛出`IOException`，我们必须捕捉到`forEach`内部的异常；因此，代码看起来很难看。通常的做法是将代码提取到一个新方法中。

```java
 public void save(Path path, List<DummyFile> files) {

      if (!Files.isDirectory(path)) {
          throw new IllegalArgumentException("Path must be a directory");
      }

      // extract it to a new method
      /*files.forEach(f -> {
            try {
                int id = f.getId();
                // create a filename
                String fileName = id + ".txt";
                Files.write(path.resolve(fileName),
                        f.getContent().getBytes(StandardCharsets.UTF_8));
            } catch (IOException e) {
                e.printStackTrace();
            }
        });*/

      // nice!
      files.forEach(f -> saveFile(path, f));

  }

  public void saveFile(Path path, DummyFile f) {
      try {
          int id = f.getId();
          // create a filename
          String fileName = id + ".txt";
          Files.write(path.resolve(fileName),
                  f.getContent().getBytes(StandardCharsets.UTF_8));
      } catch (IOException e) {
          e.printStackTrace();
      }
  } 
```

现在，我们也可以编写这样的代码:

```java
 ForEachWriteFile obj = new ForEachWriteFile();

  Path path = Paths.get("C:\\test");
  obj.createDummyFiles().forEach(o -> obj.saveFile(path, o)); 
```

## 5。forEach vs forEachOrdered

5.1`forEach`不保证流的相遇顺序，不管流是顺序的还是并行的。当以并行模式运行时，结果很明显。

```java
 Stream<String> s = Stream.of("a", "b", "c", "1", "2", "3");
  s.parallel().forEach(x -> System.out.println(x)); 
```

每次运行将产生不同的结果:

Terminal

```java
 1
2
b
c
3
a 
```

5.2`forEachOrdered`保证流的相遇顺序；因此，它牺牲了并行性的好处。

```java
 Stream<String> s = Stream.of("a", "b", "c", "1", "2", "3");
  // keep order, it is always a,b,c,1,2,3
  s.parallel().forEachOrdered(x -> System.out.println(x)); 
```

结果总是`a,b,c,1,2,3`

Terminal

```java
 a
b
c
1
2
3 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220627140650/https://github.com/mkyong/core-java)

$ cd java8

## 参考文献

*   [消费者 JavaDoc](http://web.archive.org/web/20220627140650/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/function/Consumer.html)
*   [forEach JavaDoc](http://web.archive.org/web/20220627140650/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/Stream.html#forEach(java.util.function.Consumer))
*   [Java 8 消费者示例](/web/20220627140650/https://mkyong.com/java8/java-8-consumer-examples/)
*   [在 Java 中把十六进制转换成 ASCII 码](/web/20220627140650/https://mkyong.com/java/how-to-convert-hex-to-ascii-in-java/)

<input type="hidden" id="mkyong-current-postId" value="13848">