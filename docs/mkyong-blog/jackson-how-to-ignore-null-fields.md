# Jackson——如何忽略空字段

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/jackson-how-to-ignore-null-fields/>

在 Jackson 中，我们可以使用`@JsonInclude(JsonInclude.Include.NON_NULL)`来忽略`null`字段。

*用杰克逊 2.9.8 测试的 PS*

## 1.Jackson 默认包含空字段

1.1 审查 POJO，以便稍后进行测试。

Staff.java

```java
 public class Staff {

    private String name;
    private int age;
    private String[] position;               
    private List<String> skills;            
    private Map<String, BigDecimal> salary; 
```

1.2 默认情况下，Jackson 将包含空字段。

JacksonExample.java

```java
 package com.mkyong;

import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;

public class JacksonExample {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        Staff staff = new Staff("mkyong", 38);

        try {

            String json = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(staff);

            System.out.println(json);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "position" : null,
  "skills" : null,
  "salary" : null
} 
```

要忽略`null`字段，请将`@JsonInclude`放在类级别或字段级别。

## 2.@ JSON include–类级别

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

输出

```java
 {
  "name" : "mkyong",
  "age" : 38
} 
```

## 3.@ JSON include–字段级别

Staff.java

```java
 import com.fasterxml.jackson.annotation.JsonInclude;

public class Staff {

    private String name;
    private int age;

	@JsonInclude(JsonInclude.Include.NON_NULL) //ignore null field on this property only
    private String[] position;              

	@JsonInclude(JsonInclude.Include.NON_NULL) //ignore null field on this property only
    private List<String> skills;           

    private Map<String, BigDecimal> salary; 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "salary" : null
} 
```

## 4.object mapper . setserializationinclusion

或者，我们也可以配置全局忽略空字段:

JacksonExample2.java

```java
 package com.mkyong;

import com.fasterxml.jackson.annotation.JsonInclude;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;

public class JacksonExample2 {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        Staff staff = new Staff("mkyong", 38);

        try {

            // ignore the null fields globally
            mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);

            String json = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(staff);

            System.out.println(json);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38
} 
```

## 参考

*   [杰克逊数据绑定](http://web.archive.org/web/20210815070227/https://github.com/FasterXML/jackson-databind/)
*   [Jackson 2–将 Java 对象转换成 JSON 从 JSON 转换过来](http://web.archive.org/web/20210815070227/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/)

Tags : [jackson](http://web.archive.org/web/20210815070227/https://mkyong.com/tag/jackson/) [json](http://web.archive.org/web/20210815070227/https://mkyong.com/tag/json/) [null](http://web.archive.org/web/20210815070227/https://mkyong.com/tag/null/)<input type="hidden" id="mkyong-current-postId" value="15080">