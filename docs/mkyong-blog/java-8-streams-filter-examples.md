# Java 8 流过滤器示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/java-8-tutorials/java8/java-8-streams-filter-examples/>

在本教程中，我们将向您展示几个 Java 8 示例来演示流`filter()`、`collect()`、`findAny()`和`orElse()`的使用

## 1.流过滤器()和收集()

Java 8 之前，这样过滤一个`List`:

BeforeJava8.java

```java
 package com.mkyong.java8;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class BeforeJava8 {

    public static void main(String[] args) {

        List<String> lines = Arrays.asList("spring", "node", "mkyong");
        List<String> result = getFilterOutput(lines, "mkyong");
        for (String temp : result) {
            System.out.println(temp);    //output : spring, node
        }

    }

    private static List<String> getFilterOutput(List<String> lines, String filter) {
        List<String> result = new ArrayList<>();
        for (String line : lines) {
            if (!"mkyong".equals(line)) { // we dont like mkyong
                result.add(line);
            }
        }
        return result;
    }

} 
```

输出

```java
 spring
node 
```

1.2 Java 8 中的等价例子，`stream.filter()`过滤一个`List`，`collect()`将一个流转换成一个`List`。

NowJava8.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class NowJava8 {

    public static void main(String[] args) {

        List<String> lines = Arrays.asList("spring", "node", "mkyong");

        List<String> result = lines.stream()                // convert list to stream
                .filter(line -> !"mkyong".equals(line))     // we dont like mkyong
                .collect(Collectors.toList());              // collect the output and convert streams to a List

        result.forEach(System.out::println);                //output : spring, node

    }

} 
```

输出

```java
 spring
node 
```

## 2.Streams filter()、findAny()和 orElse()

Person.java

```java
 package com.mkyong.java8;

public class Person {

    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    //gettersm setters, toString
} 
```

2.1 在 Java 8 之前，你通过名字得到一个`Person`，像这样:

BeforeJava8.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;

public class BeforeJava8 {

    public static void main(String[] args) {

        List<Person> persons = Arrays.asList(
                new Person("mkyong", 30),
                new Person("jack", 20),
                new Person("lawrence", 40)
        );

        Person result = getStudentByName(persons, "jack");
        System.out.println(result);

    }

    private static Person getStudentByName(List<Person> persons, String name) {

        Person result = null;
        for (Person temp : persons) {
            if (name.equals(temp.getName())) {
                result = temp;
            }
        }
        return result;
    }
} 
```

输出

```java
 Person{name='jack', age=20} 
```

2.2 Java 8 中的等价例子，使用`stream.filter()`过滤一个`List`，使用`.findAny().orElse (null)`返回一个有条件的对象。

NowJava8.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;

public class NowJava8 {

    public static void main(String[] args) {

        List<Person> persons = Arrays.asList(
                new Person("mkyong", 30),
                new Person("jack", 20),
                new Person("lawrence", 40)
        );

        Person result1 = persons.stream()                        // Convert to steam
                .filter(x -> "jack".equals(x.getName()))        // we want "jack" only
                .findAny()                                      // If 'findAny' then return found
                .orElse(null);                                  // If not found, return null

        System.out.println(result1);

        Person result2 = persons.stream()
                .filter(x -> "ahmook".equals(x.getName()))
                .findAny()
                .orElse(null);

        System.out.println(result2);

    }

} 
```

输出

```java
 Person{name='jack', age=20}
null 
```

2.3 适用于多种情况。

NowJava8.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;

public class NowJava8 {

    public static void main(String[] args) {

        List<Person> persons = Arrays.asList(
                new Person("mkyong", 30),
                new Person("jack", 20),
                new Person("lawrence", 40)
        );

        Person result1 = persons.stream()
                .filter((p) -> "jack".equals(p.getName()) && 20 == p.getAge())
                .findAny()
                .orElse(null);

        System.out.println("result 1 :" + result1);

        //or like this
        Person result2 = persons.stream()
                .filter(p -> {
                    if ("jack".equals(p.getName()) && 20 == p.getAge()) {
                        return true;
                    }
                    return false;
                }).findAny()
                .orElse(null);

        System.out.println("result 2 :" + result2);

    }

} 
```

输出

```java
 result 1 :Person{name='jack', age=20}
result 2 :Person{name='jack', age=20} 
```

## 3.流过滤器()和映射()

NowJava8.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class NowJava8 {

    public static void main(String[] args) {

        List<Person> persons = Arrays.asList(
                new Person("mkyong", 30),
                new Person("jack", 20),
                new Person("lawrence", 40)
        );

        String name = persons.stream()
                .filter(x -> "jack".equals(x.getName()))
                .map(Person::getName)                        //convert stream to String
                .findAny()
                .orElse("");

        System.out.println("name : " + name);

        List<String> collect = persons.stream()
                .map(Person::getName)
                .collect(Collectors.toList());

        collect.forEach(System.out::println);

    }

} 
```

输出

```java
 name : jack

mkyong
jack
lawrence 
```

**Note**
Highly recommend this Streams tutorial – [Processing Data with Java SE 8 Streams](http://web.archive.org/web/20220310233017/http://www.oracle.com/technetwork/articles/java/ma14-java-se-8-streams-2177646.html)

## 参考

1.  [Java 8 循环推理在我这里](http://web.archive.org/web/20220310233017/https://stackoverflow.com/questions/26422166/java-8-cyclic-inference-in-my-case)
2.  [Java 8 解释:使用过滤器、映射、流和 Foreach 将 Lambdas 应用于 Java 集合！](http://web.archive.org/web/20220310233017/http://zeroturnaround.com/rebellabs/java-8-explained-applying-lambdas-to-java-collections/)
3.  [Java 8 forEach 示例](http://web.archive.org/web/20220310233017/http://www.mkyong.com/java8/java-8-foreach-examples/)
4.  [Java 8 流:多重过滤器与复杂条件](http://web.archive.org/web/20220310233017/https://stackoverflow.com/questions/24054773/java-8-streams-multiple-filters-vs-complex-condition)
5.  [用 Java SE 8 流处理数据](http://web.archive.org/web/20220310233017/http://www.oracle.com/technetwork/articles/java/ma14-java-se-8-streams-2177646.html)

<input type="hidden" id="mkyong-current-postId" value="13951">