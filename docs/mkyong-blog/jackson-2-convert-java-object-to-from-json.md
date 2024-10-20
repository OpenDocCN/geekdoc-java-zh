# Jackson 2–将 Java 对象转换成 JSON 从 JSON 转换过来

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/>

在本教程中，我们将向您展示如何使用 [Jackson 2.x](http://web.archive.org/web/20221117184233/https://github.com/FasterXML/jackson-databind/) 在 Java 对象和 JSON 之间进行转换。

## 1.基础

1.1 将一个`Staff`对象转换成 from JSON。

`writeValue(...)`–Java 对象到 JSON

```java
 ObjectMapper mapper = new ObjectMapper();

	// Java object to JSON file
	mapper.writeValue(new File("c:\\test\\staff.json"), new Staff());

	// Java object to JSON string
	String jsonString = mapper.writeValueAsString(object); 
```

`readValue(...)`–JSON 到 Java 对象

```java
 ObjectMapper mapper = new ObjectMapper();

	//JSON file to Java object
	Staff obj = mapper.readValue(new File("c:\\test\\staff.json"), Staff.class);

	//JSON URL to Java object
	Staff obj = mapper.readValue(new URL("http://some-domains/api/name.json"), Staff.class);

	//JSON string to Java Object
	Staff obj = mapper.readValue("{'name' : 'mkyong'}", Staff.class); 
```

*用杰克逊 2.9.8 测试的 PS*

**Note**
Read this [How to parse JSON with Jackson](http://web.archive.org/web/20221117184233/https://www.mkyong.com/java/jackson-how-to-parse-json/), containing Jackson examples like Object to/from JSON, `@JsonView`, `@JsonProperty`, `@JsonInclude`, `@JsonIgnore`, and some FAQs.

## 1.下载杰克逊

1.1 声明`jackson-databind`，将拉入`jackson-annotations`和`jackson-core`

pom.xml

```java
 <dependency>
		<groupId>com.fasterxml.jackson.core</groupId>
		<artifactId>jackson-databind</artifactId>
		<version>2.9.8</version>
	</dependency> 
```

1.2 回顾杰克逊依赖关系:

Terminal

```java
 $ mvn dependency:tree

\- com.fasterxml.jackson.core:jackson-databind:jar:2.9.8:compile
[INFO]    +- com.fasterxml.jackson.core:jackson-annotations:jar:2.9.0:compile
[INFO]    \- com.fasterxml.jackson.core:jackson-core:jar:2.9.8:compile 
```

**Difference between Jackson 1 and Jackson 2**
Most of the APIs still maintains the same method name and signature, just the packaging is different.

*   杰克逊 1 . x–org . code Haus . Jackson . map
*   Jackson 2 . x–com . faster XML . Jackson . databind

## 2.波乔

用于测试的简单 Java 对象。

Staff.java

```java
 public class Staff {

    private String name;
    private int age;
    private String[] position;              //  Array
    private List<String> skills;            //  List
    private Map<String, BigDecimal> salary; //  Map

	// getters , setters, some boring stuff
} 
```

## 3.JSON 的 Java 对象

JacksonExample1.java

```java
 package com.mkyong;

import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.File;
import java.io.IOException;
import java.math.BigDecimal;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class JacksonExample1 {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        Staff staff = createStaff();

        try {

            // Java objects to JSON file
            mapper.writeValue(new File("c:\\test\\staff.json"), staff);

            // Java objects to JSON string - compact-print
            String jsonString = mapper.writeValueAsString(staff);

            System.out.println(jsonString);

            // Java objects to JSON string - pretty-print
            String jsonInString2 = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(staff);

            System.out.println(jsonInString2);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    private static Staff createStaff() {

        Staff staff = new Staff();

        staff.setName("mkyong");
        staff.setAge(38);
        staff.setPosition(new String[]{"Founder", "CTO", "Writer"});
        Map<String, BigDecimal> salary = new HashMap() {{
            put("2010", new BigDecimal(10000));
            put("2012", new BigDecimal(12000));
            put("2018", new BigDecimal(14000));
        }};
        staff.setSalary(salary);
        staff.setSkills(Arrays.asList("java", "python", "node", "kotlin"));

        return staff;

    }

} 
```

输出

c:\\test\\staff.json

```java
 {"name":"mkyong","age":38,"position":["Founder","CTO","Writer"],"skills":["java","python","node","kotlin"],"salary":{"2018":14000,"2012":12000,"2010":10000}} 
```

Terminal

```java
 {"name":"mkyong","age":38,"position":["Founder","CTO","Writer"],"skills":["java","python","node","kotlin"],"salary":{"2018":14000,"2012":12000,"2010":10000}}

{
  "name" : "mkyong",
  "age" : 38,
  "position" : [ "Founder", "CTO", "Writer" ],
  "skills" : [ "java", "python", "node", "kotlin" ],
  "salary" : {
    "2018" : 14000,
    "2012" : 12000,
    "2010" : 10000
  }
} 
```

## 4.JSON 到 Java 对象

JacksonExample2.java

```java
 package com.mkyong;

import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.File;
import java.io.IOException;

public class JacksonExample2 {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        try {

            // JSON file to Java object
            Staff staff = mapper.readValue(new File("c:\\test\\staff.json"), Staff.class);

            // JSON string to Java object
            String jsonInString = "{\"name\":\"mkyong\",\"age\":37,\"skills\":[\"java\",\"python\"]}";
            Staff staff2 = mapper.readValue(jsonInString, Staff.class);

            // compact print
            System.out.println(staff2);

            // pretty print
            String prettyStaff1 = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(staff2);

            System.out.println(prettyStaff1);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Staff{name='mkyong', age=37, position=null, skills=[java, python], salary=null}

{
  "name" : "mkyong",
  "age" : 37,
  "position" : null,
  "skills" : [ "java", "python" ],
  "salary" : null
} 
```

**Note**
More Jackson examples read this – [How to parse JSON with Jackson](http://web.archive.org/web/20221117184233/https://www.mkyong.com/java/jackson-how-to-parse-json/)

## 参考

*   [杰克逊数据绑定官网](http://web.archive.org/web/20221117184233/https://github.com/FasterXML/jackson-databind/)
*   [Jackson–如何解析 JSON](http://web.archive.org/web/20221117184233/https://www.mkyong.com/java/jackson-how-to-parse-json/)
*   [Gson–如何解析 JSON](http://web.archive.org/web/20221117184233/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

<input type="hidden" id="mkyong-current-postId" value="13883">