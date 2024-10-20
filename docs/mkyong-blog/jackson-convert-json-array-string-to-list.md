# Jackson–将 JSON 数组字符串转换为列表

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/jackson-convert-json-array-string-to-list/>

几个 Jackson 的例子把一个 JSON 数组字符串转换成一个`List`

```java
 // JSON array string
	// [{"name":"mkyong", "age":37}, {"name":"fong", "age":38}]

	ObjectMapper mapper = new ObjectMapper();
	String json = "[{\"name\":\"mkyong\", \"age\":37}, {\"name\":\"fong\", \"age\":38}]";

	// 1\. convert JSON array to Array objects
	Person[] pp1 = mapper.readValue(json, Person[].class);

	// 2\. convert JSON array to List of objects
	List<Person> ppl2 = Arrays.asList(mapper.readValue(json, Person[].class)); 
```

pom.xml

```java
 <dependency>
		<groupId>com.fasterxml.jackson.core</groupId>
		<artifactId>jackson-databind</artifactId>
		<version>2.9.8</version>
	</dependency> 
```

*用杰克逊 2.9.8 测试的 PS*

## 1.将 JSON 数组字符串转换为列表

1.1 JSON 数组字符串

```java
 [{"name":"mkyong", "age":37}, {"name":"fong", "age":38}] 
```

1.2 创建一个对象来映射上述 JSON 字段。

```java
 package com.mkyong;

public class Person {

    String name;
    Integer age;

    //getters and setters
} 
```

1.3 将 JSON 数组字符串转换成一个`List`

JacksonArrayExample.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class JacksonArrayExample {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();
        String json = "[{\"name\":\"mkyong\", \"age\":37}, {\"name\":\"fong\", \"age\":38}]";

        try {

            // 1\. convert JSON array to Array objects
            Person[] pp1 = mapper.readValue(json, Person[].class);

            System.out.println("JSON array to Array objects...");
            for (Person person : pp1) {
                System.out.println(person);
            }

            // 2\. convert JSON array to List of objects
            List<Person> ppl2 = Arrays.asList(mapper.readValue(json, Person[].class));

            System.out.println("\nJSON array to List of objects");
            ppl2.stream().forEach(x -> System.out.println(x));

            // 3\. alternative
            List<Person> pp3 = mapper.readValue(json, new TypeReference<List<Person>>() {});

            System.out.println("\nAlternative...");
            pp3.stream().forEach(x -> System.out.println(x));

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
} 
```

输出

```java
 1\. JSON array to Array objects...
Person{name='mkyong', age=37}
Person{name='fong', age=38}

2\. JSON array to List of objects
Person{name='mkyong', age=37}
Person{name='fong', age=38}

3\. Alternative...
Person{name='mkyong', age=37}
Person{name='fong', age=38} 
```

## 参考

*   [Jackson–将 JSON 字符串转换为映射](http://web.archive.org/web/20210814144133/https://www.mkyong.com/java/how-to-convert-java-map-to-from-json-jackson/)
*   [杰克逊数据绑定](http://web.archive.org/web/20210814144133/https://github.com/FasterXML/jackson-databind/)
*   [Jackson 2–将 Java 对象转换成 JSON 从 JSON 转换过来](http://web.archive.org/web/20210814144133/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/)

Tags : [jackson](http://web.archive.org/web/20210814144133/https://mkyong.com/tag/jackson/) [json](http://web.archive.org/web/20210814144133/https://mkyong.com/tag/json/) [json array](http://web.archive.org/web/20210814144133/https://mkyong.com/tag/json-array/) [list](http://web.archive.org/web/20210814144133/https://mkyong.com/tag/list/)<input type="hidden" id="mkyong-current-postId" value="15079">