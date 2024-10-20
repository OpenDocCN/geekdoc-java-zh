# fast Json–将 Java 对象转换成 JSON 或从 JSON 转换过来

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/fastjson-convert-java-objects-to-from-json/>

FastJson 提供了简单的 API 来将 Java 对象与 Json 相互转换

*   `JSON.toJSONString`–Java 对象到 JSON
*   `JSON.parseObject`–JSON 到 Java 对象
*   `JSON.parseArray`–Java 对象列表的 JSON 数组

**Note**
You may have interest to read this [How to parse JSON with Jackson](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-how-to-parse-json/)

总的来说，FastJson 非常简单，很容易将 Json 转换成对象，但是，它缺乏直接的`File`支持，尤其是`JSON.parseArray`方法，它需要额外的努力来读取 JSON 文件。希望将来像`parseObject`和`parseArray`这样的 API 能够直接支持像`File`或`URL`这样的源。

*PS 用 FastJson 1.2.57 测试*

pom.xml

```java
 <dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>fastjson</artifactId>
		<version>1.2.57</version>
	</dependency> 
```

## 1.波乔

一个简单的 POJO，用于 JSON 转换。

Staff.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.Date;
import java.util.List;
import java.util.Map;

public class Staff {

    private String name;
    private int age;
    private String[] position;
    private List<String> skills;
    private Map<String, BigDecimal> salary;

    //getters, setters, toString, constructor
} 
```

## 2.JSON 的 Java 对象

FastJsonExample1.java

```java
 package com.mkyong;

import com.alibaba.fastjson.JSON;

import java.io.IOException;
import java.math.BigDecimal;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.*;

public class FastJsonExample1 {

    public static void main(String[] args) {

        Staff staff = createStaff();

        // Java objects to JSON
        String json = JSON.toJSONString(staff);
        System.out.println(json);

        // Java objects to JSON, pretty-print
        String json2 = JSON.toJSONString(staff, true);
        System.out.println(json2);

        // Java objects to JSON, with formatted date
        String json3 = JSON.toJSONStringWithDateFormat(staff, "dd/MM/yyyy HH:mm:ss");
        System.out.println(json3);

        // List of Java objects to JSON Array
        List<Staff> list = Arrays.asList(createStaff(), createStaff());
        String json4 = JSON.toJSONStringWithDateFormat(list, "dd/MM/yyyy HH:mm:ss");
        System.out.println(json4);

        try {
            // can't find fastjson api to write files, np, just use the standard java.nio Files.write
            Files.write(Paths.get("c:\\projects\\staff.json"), json4.getBytes());
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
        staff.setJoinDate(new Date());

        return staff;

    }

} 
```

输出

```java
 // json
{"age":38,"joinDate":1556870430099,"name":"mkyong","position":["Founder","CTO","Writer"],
"salary":{"2018":14000,"2012":12000,"2010":10000},"skills":["java","python","node","kotlin"]}

// json2
{
	"age":38,
	"joinDate":1556870430099,
	"name":"mkyong",
	"position":["Founder","CTO","Writer"],
	"salary":{
		"2018":14000,
		"2012":12000,
		"2010":10000
	},
	"skills":[
		"java",
		"python",
		"node",
		"kotlin"
	]
}

// json3 - format date
{"age":38,"joinDate":"03/05/2019 16:00:30","name":"mkyong","position":["Founder","CTO","Writer"],
"salary":{"2018":14000,"2012":12000,"2010":10000},"skills":["java","python","node","kotlin"]}

// json4 - JSON Array
[
	{
		"age":38,
		"joinDate":1556870630615,
		"name":"mkyong",
		"position":["Founder","CTO","Writer"],
		"salary":{
			"2018":14000,
			"2012":12000,
			"2010":10000
		},
		"skills":[
			"java",
			"python",
			"node",
			"kotlin"
		]
	},
	{
		"age":38,
		"joinDate":1556870630615,
		"name":"mkyong",
		"position":["Founder","CTO","Writer"],
		"salary":{
			"2018":14000,
			"2012":12000,
			"2010":10000
		},
		"skills":[
			"java",
			"python",
			"node",
			"kotlin"
		]
	}
] 
```

## 3.JSON 到 Java 对象

FastJsonExample2.java

```java
 package com.mkyong;

import com.alibaba.fastjson.JSON;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FastJsonExample2 {

    public static void main(String[] args) {

        // JSON string to Java object
        String jsonString = "{\"name\":38,\"name\":\"mkyong\"}";
        Staff staff = JSON.parseObject(jsonString, Staff.class);

        System.out.println(staff);

        // JSON array to Java object
        String jsonArray = "[{\"name\":38,\"name\":\"mkyong\"}, {\"name\":39,\"name\":\"mkyong2\"}]";
        List<Staff> staff1 = JSON.parseArray(jsonArray, Staff.class);

        System.out.println(staff1);

        // JSON array in File to Java object
        // staff.json contain JSON array
        try (Stream<String> lines = Files.lines(Paths.get("c:\\projects\\staff.json"))) {

            String content = lines.collect(Collectors.joining());
			// Hope parseArray() will support File or Reader in future.
            List<Staff> list = JSON.parseArray(content, Staff.class);
            System.out.println(list);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
} 
```

## 参考

*   [FastJson 主页](http://web.archive.org/web/20221117050521/https://github.com/alibaba/fastjson)
*   [FastJson 用户指南](http://web.archive.org/web/20221117050521/https://github.com/alibaba/fastjson/wiki/UserGuid)
*   [FastJson 数据绑定示例](http://web.archive.org/web/20221117050521/https://github.com/alibaba/fastjson/wiki/Samples-DataBind)
*   [FastJson 最佳实践(中文)](http://web.archive.org/web/20221117050521/https://kimmking.github.io/2017/06/06/json-best-practice/)
*   【FastJson 为什么这么快？(中文)
*   [FastJson 流 API](http://web.archive.org/web/20221117050521/https://github.com/alibaba/fastjson/wiki/Stream-api)

<input type="hidden" id="mkyong-current-postId" value="15093">