# 杰克逊——如何解析 JSON

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/jackson-how-to-parse-json/>

[Jackson](http://web.archive.org/web/20221117050521/https://github.com/FasterXML/jackson-databind/) 提供`writeValue()`和`readValue()`方法将 Java 对象与 JSON 相互转换。

`mapper.writeValue`–Java 对象到 JSON

```java
 ObjectMapper mapper = new ObjectMapper();

	// Java object to JSON file
	mapper.writeValue(new File("c:\\test\\staff.json"), new Staff());

	// Java object to JSON string, default compact-print
	String jsonString = mapper.writeValueAsString(new Staff());

	// pretty-print
	String jsonString2 = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(new Staff()); 
```

`mapper.readValue`–JSON 到 Java 对象

```java
 ObjectMapper mapper = new ObjectMapper();

	//JSON file to Java object
	Staff obj = mapper.readValue(new File("c:\\test\\staff.json"), Staff.class);

	//JSON URL to Java object
	Staff obj = mapper.readValue(new URL("http://some-domains/api/staff.json"), Staff.class);

	//JSON string to Java Object
	Staff obj = mapper.readValue("{'name' : 'mkyong'}", Staff.class); 
```

*用杰克逊 2.9.8 测试的 PS*

## 1.下载杰克逊

声明`jackson-databind`，它将拉入`jackson-annotations`和`jackson-core`

pom.xml

```java
 <dependency>
		<groupId>com.fasterxml.jackson.core</groupId>
		<artifactId>jackson-databind</artifactId>
		<version>2.9.8</version>
	</dependency> 
```

Terminal

```java
 $ mvn dependency:tree

\- com.fasterxml.jackson.core:jackson-databind:jar:2.9.8:compile
[INFO]    +- com.fasterxml.jackson.core:jackson-annotations:jar:2.9.0:compile
[INFO]    \- com.fasterxml.jackson.core:jackson-core:jar:2.9.8:compile 
```

## 2.波乔

一个简单的 Java 对象 POJO，用于以后的测试。

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

## 5.@ JSON property–JSON 字段命名

5.1 违约

```java
 public class Staff {

    private String name;
	private int age; 
```

输出

```java
 {"name":"abc", "age":38} 
```

5.2 用`@JsonProperty`更改属性名称

```java
 public class Staff {

    @JsonProperty("custom_name")
    private String name;
	private int age; 
```

输出

```java
 {"custom_name":"abc", "age":38} 
```

## 6.@ JSON include–忽略空字段

默认情况下，Jackson 将包含`null`字段。

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "position" : null,
  "skills" : null,
  "salary" : null
} 
```

6.1 `@JsonInclude`类级别。

Staff.java

```java
 import com.fasterxml.jackson.annotation.JsonInclude;

											//	ignore null fields , class level
@JsonInclude(JsonInclude.Include.NON_NULL) 	//  ignore all null fields
public class Staff {

    private String name;
    private int age;
    private String[] position;              
    private List<String> skills;           
    private Map<String, BigDecimal> salary; 
	//... 
```

6.2 字段级上的`@JsonInclude`。

Staff.java

```java
 import com.fasterxml.jackson.annotation.JsonInclude;

public class Staff {

    private String name;
    private int age;

	@JsonInclude(JsonInclude.Include.NON_NULL) //ignore null field on this property only
    private String[] position;              

    private List<String> skills;           

    private Map<String, BigDecimal> salary; 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "skill" : null,
  "salary" : null
} 
```

6.3 全球。

```java
 ObjectMapper mapper = new ObjectMapper();

	// ignore all null fields globally
	mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL); 
```

**Note**
More examples for [How to ignore null fields with Jackson](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-how-to-ignore-null-fields/)

## 7.@JsonView

7.1`@JsonView`用于限制不同用户显示的字段。例如:

CompanyViews.java

```java
 package com.mkyong;

public class CompanyViews {

    public static class Normal{};

    public static class Manager extends Normal{};

} 
```

普通视图仅显示姓名和年龄，经理视图可以显示所有信息。

Staff.java

```java
 import com.fasterxml.jackson.annotation.JsonView;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;
import java.util.Map;

public class Staff {

    @JsonView(CompanyViews.Normal.class)
    private String name;

    @JsonView(CompanyViews.Normal.class)
    private int age;

    @JsonView(CompanyViews.Manager.class)
    private String[] position;

    @JsonView(CompanyViews.Manager.class)
    private List<String> skills;

    @JsonView(CompanyViews.Manager.class)
    private Map<String, BigDecimal> salary; 
```

JacksonExample.java

```java
 package com.mkyong;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.io.IOException;
import java.math.BigDecimal;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class JacksonExample {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        Staff staff = createStaff();

        try {

            // to enable pretty print
            mapper.enable(SerializationFeature.INDENT_OUTPUT);

            // normal
            String normalView = mapper
				.writerWithView(CompanyViews.Normal.class)
				.writeValueAsString(staff);

            System.out.format("Normal views\n%s\n", normalView);

            // manager
            String managerView = mapper
				.writerWithView(CompanyViews.Manager.class)
				.writeValueAsString(staff);

            System.out.format("Manager views\n%s\n", managerView);

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

```java
 Normal views
{
  "name" : "mkyong",
  "age" : 38
}

Manager views
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

**Note**
Read this [Jackson @JsonView example](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-jsonview-examples/)

## 8.@JsonIgnore 和@JsonIgnoreProperties

默认情况下，Jackson 包括所有字段，甚至包括`static`或`transient`字段。

8.1 `@JsonIgnore`忽略字段级的字段。

```java
 import com.fasterxml.jackson.annotation.JsonIgnore;

public class Staff {

    private String name;
    private int age;
    private String[] position;

    @JsonIgnore
    private List<String> skills;

    @JsonIgnore
    private Map<String, BigDecimal> salary; 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "position" : [ "Founder", "CTO", "Writer" ]
} 
```

8.2 `@JsonIgnoreProperties`忽略类级别的字段。

```java
 import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties({"salary", "position"})
public class Staff {

    private String name;
    private int age;
    private String[] position;
    private List<String> skills;
    private Map<String, BigDecimal> salary; 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "skills" : [ "java", "python", "node", "kotlin" ]
} 
```

## 9.常见问题

9.1 [将 JSON 数组字符串转换为列表](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-convert-json-array-string-to-list/)

```java
 String json = "[{\"name\":\"mkyong\", \"age\":38}, {\"name\":\"laplap\", \"age\":5}]";

	List<Staff> list = Arrays.asList(mapper.readValue(json, Staff[].class));

	// or like this:
	// List<Staff> list = mapper.readValue(json, new TypeReference<List<Staff>>(){}); 
```

9.2 [将 JSON 字符串转换为映射](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/how-to-convert-java-map-to-from-json-jackson/)

```java
 String json = "{\"name\":\"mkyong\", \"age\":\"33\"}";

	Map<String, String> map = mapper.readValue(json, Map.class);

	// or like this:
	//Map<String, String> map = mapper.readValue(json, new TypeReference<Map<String, String>>(){});

	map.forEach((k, v) -> System.out.format("[key]:%s \t[value]:%s\n", k, v)); 
```

输出

```java
 [key]:name 	[value]:mkyong
[key]:age 	[value]:33 
```

9.3 如果某个复杂的 JSON 结构不容易映射到 Java 类怎么办？
**回答:**试试 [Jackson TreeModel](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-tree-model-example/) 把 JSON 数据转换成`JsonNode`，这样我们就可以很方便的添加、更新或者删除 JSON 节点。

## 参考

*   [杰克逊数据绑定官网](http://web.archive.org/web/20221117050521/https://github.com/FasterXML/jackson-databind/)
*   [Jackson 2–将 Java 对象转换成 JSON 从 JSON 转换过来](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/)
*   [Gson–如何解析 JSON](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

<input type="hidden" id="mkyong-current-postId" value="15083">