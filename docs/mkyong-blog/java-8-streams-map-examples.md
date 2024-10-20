# Java 8 Streams map()示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-streams-map-examples/>

在 Java 8 中，`stream().map()`让你把一个对象转换成其他的东西。查看以下示例:

## 1.大写的字符串列表

1.1 将字符串列表转换成大写字母的简单 Java 示例。

TestJava8.java

```java
 package com.mkyong.java8;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class TestJava8 {

    public static void main(String[] args) {

        List<String> alpha = Arrays.asList("a", "b", "c", "d");

        //Before Java8
        List<String> alphaUpper = new ArrayList<>();
        for (String s : alpha) {
            alphaUpper.add(s.toUpperCase());
        }

        System.out.println(alpha); //[a, b, c, d]
        System.out.println(alphaUpper); //[A, B, C, D]

        // Java 8
        List<String> collect = alpha.stream().map(String::toUpperCase).collect(Collectors.toList());
        System.out.println(collect); //[A, B, C, D]

        // Extra, streams apply to any data type.
        List<Integer> num = Arrays.asList(1,2,3,4,5);
        List<Integer> collect1 = num.stream().map(n -> n * 2).collect(Collectors.toList());
        System.out.println(collect1); //[2, 4, 6, 8, 10]

    }

} 
```

## 2.对象列表->字符串列表

2.1 从`staff`对象列表中获取所有的`name`值。

Staff.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;

public class Staff {

    private String name;
    private int age;
    private BigDecimal salary;
	//...
} 
```

TestJava8.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class TestJava8 {

    public static void main(String[] args) {

        List<Staff> staff = Arrays.asList(
                new Staff("mkyong", 30, new BigDecimal(10000)),
                new Staff("jack", 27, new BigDecimal(20000)),
                new Staff("lawrence", 33, new BigDecimal(30000))
        );

        //Before Java 8
        List<String> result = new ArrayList<>();
        for (Staff x : staff) {
            result.add(x.getName());
        }
        System.out.println(result); //[mkyong, jack, lawrence]

        //Java 8
        List<String> collect = staff.stream().map(x -> x.getName()).collect(Collectors.toList());
        System.out.println(collect); //[mkyong, jack, lawrence]

    }

} 
```

## 3.对象列表->其他对象列表

3.1 这个例子向你展示了如何将一列`staff`对象转换成一列`StaffPublic`对象。

Staff.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;

public class Staff {

    private String name;
    private int age;
    private BigDecimal salary;
	//...
} 
```

StaffPublic.java

```java
 package com.mkyong.java8;

public class StaffPublic {

    private String name;
    private int age;
    private String extra;
    //...
} 
```

Java 8 之前的 3.2。

BeforeJava8.java

```java
 package com.mkyong.java8;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class BeforeJava8 {

    public static void main(String[] args) {

        List<Staff> staff = Arrays.asList(
                new Staff("mkyong", 30, new BigDecimal(10000)),
                new Staff("jack", 27, new BigDecimal(20000)),
                new Staff("lawrence", 33, new BigDecimal(30000))
        );

        List<StaffPublic> result = convertToStaffPublic(staff);
        System.out.println(result);

    }

    private static List<StaffPublic> convertToStaffPublic(List<Staff> staff) {

        List<StaffPublic> result = new ArrayList<>();

        for (Staff temp : staff) {

            StaffPublic obj = new StaffPublic();
            obj.setName(temp.getName());
            obj.setAge(temp.getAge());
            if ("mkyong".equals(temp.getName())) {
                obj.setExtra("this field is for mkyong only!");
            }

            result.add(obj);
        }

        return result;

    }

} 
```

输出

```java
 [
	StaffPublic{name='mkyong', age=30, extra='this field is for mkyong only!'}, 
	StaffPublic{name='jack', age=27, extra='null'}, 
	StaffPublic{name='lawrence', age=33, extra='null'}
] 
```

3.3 Java 8 示例。

NowJava8.java

```java
 package com.mkyong.java8;

package com.hostingcompass.web.java8;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class NowJava8 {

    public static void main(String[] args) {

        List<Staff> staff = Arrays.asList(
                new Staff("mkyong", 30, new BigDecimal(10000)),
                new Staff("jack", 27, new BigDecimal(20000)),
                new Staff("lawrence", 33, new BigDecimal(30000))
        );

		// convert inside the map() method directly.
        List<StaffPublic> result = staff.stream().map(temp -> {
            StaffPublic obj = new StaffPublic();
            obj.setName(temp.getName());
            obj.setAge(temp.getAge());
            if ("mkyong".equals(temp.getName())) {
                obj.setExtra("this field is for mkyong only!");
            }
            return obj;
        }).collect(Collectors.toList());

        System.out.println(result);

    }

} 
```

输出

```java
 [
	StaffPublic{name='mkyong', age=30, extra='this field is for mkyong only!'}, 
	StaffPublic{name='jack', age=27, extra='null'}, 
	StaffPublic{name='lawrence', age=33, extra='null'}
] 
```

## 参考

1.  [用 Java SE 8 流处理数据，第 1 部分](http://web.archive.org/web/20221207065707/http://www.oracle.com/technetwork/articles/java/ma14-java-se-8-streams-2177646.html)
2.  [Java 8–过滤地图示例](http://web.archive.org/web/20221207065707/https://www.mkyong.com/java8/java-8-filter-a-map-examples/)
3.  [Java 8 平面图示例](http://web.archive.org/web/20221207065707/https://www.mkyong.com/java8/java-8-flatmap-example/)
4.  [收藏家 JavaDoc](http://web.archive.org/web/20221207065707/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)

<input type="hidden" id="mkyong-current-postId" value="14528">