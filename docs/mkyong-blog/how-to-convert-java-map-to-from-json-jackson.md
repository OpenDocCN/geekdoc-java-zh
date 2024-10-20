# Jackson——将 JSON 字符串转换成映射

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/how-to-convert-java-map-to-from-json-jackson/>

在 Jackson 中，我们可以使用`mapper.readValue(json, Map.class)`将 JSON 字符串转换成`Map`

*用杰克逊 2.9.8 测试的 PS*

pom.xml

```java
 <dependency>
		<groupId>com.fasterxml.jackson.core</groupId>
		<artifactId>jackson-databind</artifactId>
		<version>2.9.8</version>
	</dependency> 
```

## 1.要映射的 JSON 字符串

JacksonMapExample1.java

```java
 package com.mkyong;

import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;
import java.util.Map;

public class JacksonMapExample1 {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();
        String json = "{\"name\":\"mkyong\", \"age\":\"37\"}";

        try {

            // convert JSON string to Map
            Map<String, String> map = mapper.readValue(json, Map.class);

			// it works
            //Map<String, String> map = mapper.readValue(json, new TypeReference<Map<String, String>>() {});

            System.out.println(map);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
} 
```

输出

```java
 {name=mkyong, age=37} 
```

## 2.映射到 JSON 字符串

JacksonMapExample2.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.util.HashMap;
import java.util.Map;

public class JacksonMapExample2 {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        Map<String, String> map = new HashMap<>();
        map.put("name", "mkyong");
        map.put("age", "37");

        try {

            // convert map to JSON string
            String json = mapper.writeValueAsString(map);

            System.out.println(json);   // compact-print

            json = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(map);

            System.out.println(json);   // pretty-print

        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }

    }
} 
```

输出

```java
 {"name":"mkyong","age":"37"}
{
  "name" : "mkyong",
  "age" : "37"
} 
```

## 3.要映射的 JSON 数组？

3.1 JSON 数组字符串是这样的

```java
 [{"age":29,"name":"mkyong"}, {"age":30,"name":"fong"}] 
```

它应该转换成一个`List`，而不是一个`Map`，例如:

```java
 // convert JSON array to List
	List<Person> list = Arrays.asList(mapper.readValue(json, Person[].class)); 
```

**Note**
Read this [Jackson – Convert JSON array string to List](http://web.archive.org/web/20220612145935/https://www.mkyong.com/java/jackson-convert-json-array-string-to-list/)

## 参考

*   [Jackson–将 JSON 数组字符串转换为列表](http://web.archive.org/web/20220612145935/https://www.mkyong.com/java/jackson-convert-json-array-string-to-list/)
*   [杰克逊数据绑定](http://web.archive.org/web/20220612145935/https://github.com/FasterXML/jackson-databind/)
*   [Jackson 2–将 Java 对象转换成 JSON 从 JSON 转换过来](http://web.archive.org/web/20220612145935/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/)

<input type="hidden" id="mkyong-current-postId" value="9981">