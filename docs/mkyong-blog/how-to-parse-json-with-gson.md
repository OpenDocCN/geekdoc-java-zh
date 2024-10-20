# gson–如何解析 JSON

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/how-to-parse-json-with-gson/>

[Gson](http://web.archive.org/web/20221117184204/https://github.com/google/gson) 提供简单的`toJson()`和`fromJson()`方法，将 Java 对象转换成 JSON 对象或从 JSON 对象转换过来。

`toJson()`–JSON 的 Java 对象

```java
 Gson gson = new Gson();

	// 1\. Java object to JSON file
	gson.toJson(obj, new FileWriter("C:\\fileName.json"));

	// 2\. Java object to JSON string
	String json = gson.toJson(obj); 
```

`fromJson()`–JSON 到 Java 对象

```java
 Gson gson = new Gson();

	// 1\. JSON file to Java object
	Object object = gson.fromJson(new FileReader("C:\\fileName.json"), Object.class);

	// 2\. JSON string to Java object
	String json = "{'name' : 'mkyong'}";
	Object object = gson.fromJson(json, Staff.class); 
```

## 1.下载 Gson

pom.xml

```java
 <dependency>
		<groupId>com.google.code.gson</groupId>
		<artifactId>gson</artifactId>
		<version>2.8.5</version>
	</dependency> 
```

## 2.Java 对象

以便以后测试。

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

## 3.JSON 的 Java 对象

3.1 在 Gson 中，我们可以使用`gson.toJson()`将 Java 对象转换成 JSON。

GsonExample1.java

```java
 package com.mkyong;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

import java.io.FileWriter;
import java.io.IOException;
import java.math.BigDecimal;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class GsonExample1 {

    public static void main(String[] args) {

        // pretty print
        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        Staff staff = createStaffObject();

        // Java objects to String
        String json = gson.toJson(staff);

        //System.out.println(json);

        // Java objects to File
        try (FileWriter writer = new FileWriter("c:\\test\\staff.json")) {
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

输出

c:\\test\\staff.json

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

        try (Reader reader = new FileReader("c:\\test\\staff.json")) {

            // Convert JSON File to Java Object
            Staff staff = gson.fromJson(reader, Staff.class);

			// print staff 
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

## 5.漂亮的打印 JSON

5.1 默认的 JSON 输出是压缩模式。

GsonExample3.java

```java
 package com.mkyong;

import com.google.gson.Gson;

public class GsonExample3 {

    public static void main(String[] args) {

        // compact print
        Gson gson = new Gson();

        String[] lang = {"Java", "Node", "Kotlin", "JavaScript"};
        String json = gson.toJson(lang);

        System.out.println(json);

    }

} 
```

输出

```java
 ["Java","Node","Kotlin","JavaScript"] 
```

5.2 启用漂亮打印。

GsonExample4.java

```java
 package com.mkyong;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class GsonExample4 {

    public static void main(String[] args) {

        // pretty print
        Gson gson = new GsonBuilder().setPrettyPrinting().create();

        String[] lang = {"Java", "Node", "Kotlin", "JavaScript"};
        String json = gson.toJson(lang);

        System.out.println(json);

    }

} 
```

输出

```java
 [
  "Java",
  "Node",
  "Kotlin",
  "JavaScript"
] 
```

## 6.排除字段

在 Gson 中，有很多方法可以排除某些字段。

6.1 默认情况下，`transient`和`static`字段将被排除。我们可以通过`excludeFieldsWithModifiers`覆盖默认设置

如果我们只想排除静态字段。

```java
 import java.lang.reflect.Modifier;

	Gson gson = new GsonBuilder()
			.excludeFieldsWithModifiers(Modifier.STATIC)
			.create(); 
```

如果我们想排除瞬态和静态字段，默认。

```java
 import java.lang.reflect.Modifier;

	Gson gson = new GsonBuilder()
			.excludeFieldsWithModifiers(Modifier.STATIC, Modifier.TRANSIENT)
			.create(); 
```

在这种配置中，将包括静态和瞬态场。

```java
 Gson gson = new GsonBuilder()
			.excludeFieldsWithModifiers()
			.create(); 
```

6.2 通过`@Expose`排除字段

`@Expose`定义了从 JSON 的序列化和反序列化中排除哪些字段。要使用`@Expose`，我们需要像这样创建 Gson 对象:

```java
 Gson gson = new GsonBuilder()
			.excludeFieldsWithoutExposeAnnotation()
			.create(); 
```

如果`.excludeFieldsWithoutExposeAnnotation()`模式被启用，所有没有`@Expose`的字段将被排除。举个例子，

```java
 import com.google.gson.annotations.Expose;

public class Staff {

    @Expose(serialize = true, deserialize = true)
    private String name;
    @Expose
    private int age;
    @Expose(serialize = false, deserialize = true)

    private String[] position;              
    private List<String> skills;            
    private Map<String, BigDecimal> salary; 
```

如果将上面的 Java 对象转换成 JSON，输出将是这样的

```java
 {
  "name": "mkyong",
  "age": 35
} 
```

6.3 根据`ExclusionStrategies`、注释、类型、字段名等排除字段。Gson 是灵活的。

自定义注释`@ExcludeField`

ExcludeField.java

```java
 package com.mkyong;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD})
public @interface ExcludeField {
} 
```

一个`ExclusionStrategy`来定义哪些字段应该被排除或跳过。

CustomExclusionStrategy.java

```java
 package com.mkyong;

import com.google.gson.ExclusionStrategy;
import com.google.gson.FieldAttributes;

public class CustomExclusionStrategy implements ExclusionStrategy {

    private final Class<?> typeToSkip;

    public CustomExclusionStrategy(Class<?> typeToSkip) {
        this.typeToSkip = typeToSkip;
    }

    @Override
    public boolean shouldSkipField(FieldAttributes f) {

        // if field name 'salary`, skip
        if ("salary".equals(f.getName())) {
            return true;
        }

        // if found @ExcludeField, skip
        if (f.getAnnotation(ExcludeField.class) != null) {
            return true;
        }

        return false;
    }

    @Override
    public boolean shouldSkipClass(Class<?> clazz) {
        return (clazz == typeToSkip);
    }

} 
```

再次查看`staff`对象。

Staff.java

```java
 public class Staff {

    private String name;
    private int age;
    @ExcludeField
    private String[] position;
    private List<String> skills;
    private Map<String, BigDecimal> salary; 
```

启用`ExclusionStrategy`模式。

```java
 Gson gson = new GsonBuilder()
		.setExclusionStrategies(new CustomExclusionStrategy(List.class)) // exclude all List fields.
		.create(); 
```

输出，本例中，字段名`salary`、`@ExcludeField`字段和`List`类型字段将被排除。

```java
 {"name":"mkyong","age":35} 
```

## 7.空对象支持

`null`对象字段被忽略。

GsonExample5.java

```java
 package com.mkyong;

import com.google.gson.Gson;

public class GsonExample5 {

    public static void main(String[] args) {

        Gson gson = new Gson();

        Staff staff = createStaffObject();

        String json = gson.toJson(staff);

        System.out.println(json);

    }

    private static Staff createStaffObject() {

        Staff staff = new Staff();

        staff.setName("mkyong");
        staff.setAge(35);

        return staff;

    }

} 
```

输出

```java
 {"name":"mkyong","age":35} 
```

显示空值。

```java
 Gson gson = new GsonBuilder().serializeNulls().create(); 
```

输出

```java
 {"name":"mkyong","age":35,"position":null,"skills":null,"salary":null} 
```

## 8.JSON 字段命名支持

默认

```java
 public class Staff {

    private String name; 
```

输出

```java
 {"name":"abc"} 
```

用`@SerializedName`自定义字段名

```java
 public class Staff {

    @SerializedName("mkyong_name")
    private String name; 
```

输出

```java
 {"mkyong_name":"abc"} 
```

## 9.版本支持

```java
 import com.google.gson.annotations.Since;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;
import java.util.Map;

public class Staff {

    @Since(1.0)
    private String name;

    @Since(2.0)
    private int age;

    @Since(3.0)
    private String[] position;

    private List<String> skills;
    private Map<String, BigDecimal> salary; 
```

在本例中，字段`position`(版本 3)将被排除。

```java
 Gson gson = new GsonBuilder()
                .serializeNulls()
                .setVersion(2.0) // version <= 2.0 will be included.
                .create(); 
```

输出

```java
 {"name":null,"age":0,"skills":null,"salary":null} 
```

## 10.常见问题

一些常见的问题。

10.1 使用`TypeToken`将 JSON 数组转换为对象列表

GsonExample4.java

```java
 package com.mkyong;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

import java.util.List;

public class GsonExample4 {

    public static void main(String[] args) {

        Gson gson = new Gson();
        String json = "[{\"name\":\"mkyong\"}, {\"name\":\"laplap\"}]";
        List<Staff> list = gson.fromJson(json, new TypeToken<List<Staff>>() {}.getType());
        list.forEach(x -> System.out.println(x));

    }

} 
```

输出

```java
 Staff{name='mkyong', age=0, position=null, skills=null, salary=null}
Staff{name='laplap', age=0, position=null, skills=null, salary=null} 
```

10.2 将一个 JSON 转换成一个`Map`

GsonExample5.java

```java
 package com.mkyong;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

import java.util.Map;

public class GsonExample5 {

    public static void main(String[] args) {

        Gson gson = new Gson();

        String json = "{\"name\":\"mkyong\", \"age\":33}";
        Map<String, Object> map = gson.fromJson(json, new TypeToken<Map<String, Object>>() {}.getType());
        map.forEach((x, y) -> System.out.println("key : " + x + " , value : " + y));

    }

} 
```

输出

```java
 key : name , value : mkyong
key : age , value : 33.0 
```

**Note**
Read more [Gson user guide](http://web.archive.org/web/20221117184204/https://github.com/google/gson/blob/master/UserGuide.md)

## 参考

*   [Gson github 链接](http://web.archive.org/web/20221117184204/https://github.com/google/gson)
*   [如何将 Java 对象转换为 JSON (Gson)](http://web.archive.org/web/20221117184204/https://www.mkyong.com/java/how-do-convert-java-object-to-from-json-format-gson-api/)

<input type="hidden" id="mkyong-current-postId" value="15076">