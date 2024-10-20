# 如何在 Java 对象和 JSON 之间转换(Jackson)

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/how-to-convert-java-object-to-from-json-jackson/>

在本教程中，我们将向您展示如何使用 Jackson 1.x 数据绑定在 Java 对象和 JSON 之间进行转换。

**Note**
Jackson 1.x is a maintenance project, please use [Jackson 2.x](http://web.archive.org/web/20220802150259/https://github.com/FasterXML/jackson) instead.**Note**
This tutorial is obsolete, no more update, please refer to the latest [Jackson 2 tutorial – Object to / from JSON](http://web.archive.org/web/20220802150259/http://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/).

## 1.快速参考

1.1 将 Java 对象转换成 JSON，`writeValue(...)`

```java
 ObjectMapper mapper = new ObjectMapper();
User user = new User();

//Object to JSON in file
mapper.writeValue(new File("c:\\user.json"), user);

//Object to JSON in String
String jsonInString = mapper.writeValueAsString(user); 
```

1.2 将 JSON 转换成 Java 对象，`readValue(...)`

```java
 ObjectMapper mapper = new ObjectMapper();
String jsonInString = "{'name' : 'mkyong'}";

//JSON from file to Object
User user = mapper.readValue(new File("c:\\user.json"), User.class);

//JSON from String to Object
User user = mapper.readValue(jsonInString, User.class); 
```

所有的例子都用杰克逊 1.9.13 进行了测试

## 2.杰克逊依赖

对于 Jackson 1.x，它包含 6 个不同用途的独立 jar，在大多数情况下，你只需要`jackson-mapper-asl`。

pom.xml

```java
 <dependency>
		<groupId>org.codehaus.jackson</groupId>
		<artifactId>jackson-mapper-asl</artifactId>
		<version>1.9.13</version>
	</dependency> 
```

## 3.POJO(普通旧 Java 对象)

用于测试的用户对象。

User.java

```java
 package com.mkyong.json;

import java.util.List;

public class User {

	private String name;
	private int age;
	private List<String> messages;

	//getters and setters
} 
```

## 4.JSON 的 Java 对象

将一个`user`对象转换成 JSON 格式的字符串。

JacksonExample.java

```java
 package com.mkyong.json;

import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import org.codehaus.jackson.JsonGenerationException;
import org.codehaus.jackson.map.JsonMappingException;
import org.codehaus.jackson.map.ObjectMapper;

public class JacksonExample {
	public static void main(String[] args) {

		ObjectMapper mapper = new ObjectMapper();

		//For testing
		User user = createDummyUser();

		try {
			//Convert object to JSON string and save into file directly 
			mapper.writeValue(new File("D:\\user.json"), user);

			//Convert object to JSON string
			String jsonInString = mapper.writeValueAsString(user);
			System.out.println(jsonInString);

			//Convert object to JSON string and pretty print
			jsonInString = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(user);
			System.out.println(jsonInString);

		} catch (JsonGenerationException e) {
			e.printStackTrace();
		} catch (JsonMappingException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

	private static User createDummyUser(){

		User user = new User();

		user.setName("mkyong");
		user.setAge(33);

		List<String> msg = new ArrayList<>();
		msg.add("hello jackson 1");
		msg.add("hello jackson 2");
		msg.add("hello jackson 3");

		user.setMessages(msg);

		return user;

	}
} 
```

输出

```java
 //new json file is created in D:\\user.json"

{"name":"mkyong","age":33,"messages":["hello jackson 1","hello jackson 2","hello jackson 3"]}

{
  "name" : "mkyong",
  "age" : 33,
  "messages" : [ "hello jackson 1", "hello jackson 2", "hello jackson 3" ]
} 
```

## 5.JSON 到 Java 对象

读取 JSON 字符串并将其转换回 Java 对象。

JacksonExample.java

```java
 package com.mkyong.json;

import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import org.codehaus.jackson.JsonGenerationException;
import org.codehaus.jackson.map.JsonMappingException;
import org.codehaus.jackson.map.ObjectMapper;

public class JacksonExample {
	public static void main(String[] args) {

		ObjectMapper mapper = new ObjectMapper();

		try {

			// Convert JSON string from file to Object
			User user = mapper.readValue(new File("G:\\user.json"), User.class);
			System.out.println(user);

			// Convert JSON string to Object
			String jsonInString = "{\"age\":33,\"messages\":[\"msg 1\",\"msg 2\"],\"name\":\"mkyong\"}";
			User user1 = mapper.readValue(jsonInString, User.class);
			System.out.println(user1);

		} catch (JsonGenerationException e) {
			e.printStackTrace();
		} catch (JsonMappingException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

} 
```

输出

```java
 User [name=mkyong, age=33, messages=[hello jackson 1, hello jackson 2, hello jackson 3]]

User [name=mkyong, age=33, messages=[msg 1, msg 2]] 
```

## 6.@JsonView

从 1.4 版本开始，Jackson 就支持这个功能，它可以让你控制显示哪些字段。

6.1 一个简单的类。

Views.java

```java
 package com.mkyong.json;

public class Views {

	public static class NameOnly{};
	public static class AgeAndName extends NameOnly{};

} 
```

6.2 在您想要显示的字段上进行注释。

User.java

```java
 package com.mkyong.json;

import java.util.List;
import org.codehaus.jackson.map.annotate.JsonView;

public class User {

	@JsonView(Views.NameOnly.class)
	private String name;

	@JsonView(Views.AgeAndName.class)
	private int age;

	private List<String> messages;

	//getter and setters
} 
```

6.3 通过`writerWithView()`使能`@JsonView`。

JacksonExample.java

```java
 package com.mkyong.json;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import org.codehaus.jackson.JsonGenerationException;
import org.codehaus.jackson.map.JsonMappingException;
import org.codehaus.jackson.map.ObjectMapper;
import org.codehaus.jackson.map.SerializationConfig;

public class JacksonExample {
	public static void main(String[] args) {

		ObjectMapper mapper = new ObjectMapper();
		//By default all fields without explicit view definition are included, disable this
		mapper.configure(SerializationConfig.Feature.DEFAULT_VIEW_INCLUSION, false);

		//For testing
		User user = createDummyUser();

		try {
			//display name only
			String jsonInString = mapper.writerWithView(Views.NameOnly.class).writeValueAsString(user);
			System.out.println(jsonInString);

			//display namd ana age
			jsonInString = mapper.writerWithView(Views.AgeAndName.class).writeValueAsString(user);
			System.out.println(jsonInString);

		} catch (JsonGenerationException e) {
			e.printStackTrace();
		} catch (JsonMappingException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

	private static User createDummyUser(){

		User user = new User();

		user.setName("mkyong");
		user.setAge(33);

		List<String> msg = new ArrayList<>();
		msg.add("hello jackson 1");
		msg.add("hello jackson 2");
		msg.add("hello jackson 3");

		user.setMessages(msg);

		return user;

	}
} 
```

输出

```java
 {"name":"mkyong"}
{"name":"mkyong","age":33} 
```

## 参考

1.  [杰克逊项目主页@github](http://web.archive.org/web/20220802150259/https://github.com/FasterXML/jackson")
2.  [Gson–将 Java 对象转换成 JSON 从 JSON 转换过来](http://web.archive.org/web/20220802150259/http://www.mkyong.com/java/how-do-convert-java-object-to-from-json-format-gson-api/)

<input type="hidden" id="mkyong-current-postId" value="9940">