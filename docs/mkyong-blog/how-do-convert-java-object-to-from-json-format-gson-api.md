# gson——如何在 Java 对象和 JSON 之间进行转换

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/how-do-convert-java-object-to-from-json-format-gson-api/>

在本教程中，我们将向您展示如何使用 [Gson](http://web.archive.org/web/20221117051516/https://github.com/google/gson) 在 Java 对象和 JSON 之间进行转换。

页（page 的缩写）所有的例子都经过 Gson 2.8.5 的测试

**Note**
JSON stands for JavaScript Object Notation, it is a lightweight data-interchange format. You can see many Java applications started to throw away XML format and start using JSON as a new data-interchange format. Java is all about object, often times, you need to convert an object into JSON format for data-interchange or vice verse.**Note**
Jackson is another high performance JSON processor, try this [Jackson 2 – Java object to / from JSON](http://web.archive.org/web/20221117051516/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/)

## 1.下载 Gson

pom.xml

```java
 <dependency>
		<groupId>com.google.code.gson</groupId>
		<artifactId>gson</artifactId>
		<version>2.8.5</version>
	</dependency> 
```

## 2.Gson 基本

`toJson()`–将 Java 对象转换为 JSON

```java
 Gson gson = new Gson();

	Staff obj = new Staff();

	// 1\. Java object to JSON file
	gson.toJson(obj, new FileWriter("C:\\projects\\staff.json"));

	// 2\. Java object to JSON string
	String jsonInString = gson.toJson(obj); 
```

`fromJson()`–将 JSON 转换成 Java 对象

```java
 Gson gson = new Gson();

	// 1\. JSON file to Java object
	Staff staff = gson.fromJson(new FileReader("C:\\projects\\staff.json"), Staff.class);

	// 2\. JSON string to Java object
	String json = "{'name' : 'mkyong'}";
	Staff staff = gson.fromJson(json, Staff.class);

	// 3\. JSON file to JsonElement, later String
	JsonElement json = gson.fromJson(new FileReader("C:\\projects\\staff.json"), JsonElement.class);
    String result = gson.toJson(json); 
```

## 3.JSON 的 Java 对象

3.1 一个 Java POJO，后来用这个进行转换。

Staff.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;
import java.util.Map;

public class Staff {

    private String name;
    private int age;
    private String[] position;              // array
    private List<String> skills;            // list
    private Map<String, BigDecimal> salary; // map

    //getters and setters
} 
```

3.2 在 Gson 中，我们可以使用`gson.toJson()`将 Java 对象转换成 JSON。

GsonExample1.java

```java
 package com.mkyong;

import com.google.gson.Gson;

import java.io.FileWriter;
import java.io.IOException;
import java.math.BigDecimal;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class GsonExample1 {

    public static void main(String[] args) {

        Gson gson = new Gson();

        Staff staff = createStaffObject();

        // Java objects to String
        // String json = gson.toJson(staff);

        // Java objects to File
        try (FileWriter writer = new FileWriter("C:\\projects\\staff.json")) {
            gson.toJson(staff, writer);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    private static Staff createStaffObject() {

        Staff staff = new Staff();

        staff.setName("mkyong");
        staff.setAge(35);
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

默认情况下，Gson 以紧凑模式编写 JSON。

C:\\projects\\staff.json

```java
 {"name":"mkyong","age":35,"position":["Founder","CTO","Writer"],"skills":["java","python","node","kotlin"],"salary":{"2018":14000,"2012":12000,"2010":10000}} 
```

要启用漂亮打印模式:

```java
 import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

	Gson gson = new GsonBuilder().setPrettyPrinting().create(); 
```

输出

C:\\projects\\staff.json

```java
 {
  "name": "mkyong",
  "age": 35,
  "position": [
    "Founder",
    "CTO",
    "Writer"
  ],
  "skills": [
    "java",
    "python",
    "node",
    "kotlin"
  ],
  "salary": {
    "2018": 14000,
    "2012": 12000,
    "2010": 10000
  }
} 
```

## 4.JSON 到 Java 对象

4.1 在 Gson 中，我们可以使用`gson.fromJson`将 JSON 转换回 Java 对象。

GsonExample2.java

```java
 package com.mkyong;

import com.google.gson.Gson;

import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;

public class GsonExample2 {

    public static void main(String[] args) {

        Gson gson = new Gson();

        try (Reader reader = new FileReader("c:\\projects\\staff.json")) {

            // Convert JSON File to Java Object
            Staff staff = gson.fromJson(reader, Staff.class);

			// print staff object
            System.out.println(staff);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 Staff{name='mkyong', age=35, position=[Founder, CTO, Writer], skills=[java, python, node, kotlin], salary={2018=14000, 2012=12000, 2010=10000}} 
```

4.2 转换为`JsonElement`

GsonExample3.java

```java
 package com.mkyong;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonElement;

import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;

public class GsonExample3 {

    public static void main(String[] args) {

		// pretty print 
        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        try (Reader reader = new FileReader("c:\\projects\\staff.json")) {

            // Convert JSON to JsonElement, and later to String
            JsonElement json = gson.fromJson(reader, JsonElement.class);

            String jsonInString = gson.toJson(json);

            System.out.println(jsonInString);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 {
  "name": "mkyong",
  "age": 35,
  "position": [
    "Founder",
    "CTO",
    "Writer"
  ],
  "skills": [
    "java",
    "python",
    "node",
    "kotlin"
  ],
  "salary": {
    "2018": 14000,
    "2012": 12000,
    "2010": 10000
  }
} 
```

**Note**
More [Gson examples](http://web.archive.org/web/20221117051516/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

## 参考

*   [Gson github 链接](http://web.archive.org/web/20221117051516/https://github.com/google/gson)
*   [Gson–如何解析 JSON](http://web.archive.org/web/20221117051516/https://www.mkyong.com/java/how-to-parse-json-with-gson/)
*   [JSON 官方网站](http://web.archive.org/web/20221117051516/https://www.json.org/)
*   [维基百科–JSON](http://web.archive.org/web/20221117051516/https://en.wikipedia.org/wiki/JSON)
*   [Gson–如何启用 pretty print JSON 输出](http://web.archive.org/web/20221117051516/https://www.mkyong.com/java/how-to-enable-pretty-print-json-output-gson/)

<input type="hidden" id="mkyong-current-postId" value="1141">