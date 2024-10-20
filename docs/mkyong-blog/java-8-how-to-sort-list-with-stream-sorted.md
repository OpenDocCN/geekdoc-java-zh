# Java 8–如何使用 stream.sorted()对列表进行排序

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-sort-list-with-stream-sorted/>

几个例子告诉你如何用`stream.sorted()`对`List`排序

## 1.目录

1.1 用`Comparator.naturalOrder()`对列表进行排序

```java
 package com.mkyong.sorted;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamApplication {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("9", "A", "Z", "1", "B", "Y", "4", "a", "c");

        /* 
		List<String> sortedList = list.stream()
			.sorted(Comparator.naturalOrder())
			.collect(Collectors.toList());

        List<String> sortedList = list.stream()
			.sorted((o1,o2)-> o1.compareTo(o2))
			.collect(Collectors.toList());
		*/

		List<String> sortedList = list.stream().sorted().collect(Collectors.toList());

        sortedList.forEach(System.out::println);

    }
} 
```

输出

```java
 1
4
9
A
B
Y
Z
a
c 
```

1.2 用`Comparator.reverseOrder()`对列表进行排序

```java
 package com.mkyong.sorted;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class StreamApplication {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("9", "A", "Z", "1", "B", "Y", "4", "a", "c");

        /*
		List<String> sortedList = list.stream()
			.sorted((o1,o2)-> o2.compareTo(o1))
			.collect(Collectors.toList());
		*/

        List<String> sortedList = list.stream()
			.sorted(Comparator.reverseOrder())
			.collect(Collectors.toList());

        sortedList.forEach(System.out::println);

    }
} 
```

输出

```java
 c
a
Z
Y
B
A
9
4
1 
```

## 2.列出对象

1.1 按年龄排序，自然顺序。

```java
 package com.mkyong.sorted;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class StreamApplication {

    static List<User> users = Arrays.asList(
            new User("C", 30),
            new User("D", 40),
            new User("A", 10),
            new User("B", 20),
            new User("E", 50));

    public static void main(String[] args) {

        /*List<User> sortedList = users.stream()
			.sorted((o1, o2) -> o1.getAge() - o2.getAge())
			.collect(Collectors.toList());*/

        List<User> sortedList = users.stream()
			.sorted(Comparator.comparingInt(User::getAge))
			.collect(Collectors.toList());

        sortedList.forEach(System.out::println);

    }

    static class User {

        private String name;
        private int age;

        public User(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        @Override
        public String toString() {
            return "User{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }
} 
```

输出

```java
 User{name='A', age=10}
User{name='B', age=20}
User{name='C', age=30}
User{name='D', age=40}
User{name='E', age=50} 
```

1.2 逆序。

```java
 List<User> sortedList = users.stream()
		.sorted(Comparator.comparingInt(User::getAge)
		.reversed())
		.collect(Collectors.toList());

    sortedList.forEach(System.out::println); 
```

输出

```java
 User{name='E', age=50}
User{name='D', age=40}
User{name='C', age=30}
User{name='B', age=20}
User{name='A', age=10} 
```

1.3 按名称排序

```java
 /*List<User> sortedList = users.stream()
		.sorted((o1, o2) -> o1.getName().compareTo(o2.getName()))
		.collect(Collectors.toList());*/

	List<User> sortedList = users.stream()
		.sorted(Comparator.comparing(User::getName))
		.collect(Collectors.toList()); 
```

# 参考

*   [Java 8 Lambda:比较器示例](http://web.archive.org/web/20220627140650/https://www.mkyong.com/java8/java-8-lambda-comparator-example/)
*   [Java 8–如何对地图进行排序](http://web.archive.org/web/20220627140650/https://www.mkyong.com/java8/java-8-how-to-sort-a-map/)
*   [流式排序文档](http://web.archive.org/web/20220627140650/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#sorted-java.util.Comparator-)

<input type="hidden" id="mkyong-current-postId" value="14954">