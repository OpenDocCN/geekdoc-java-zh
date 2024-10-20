# Jackson——如何实现漂亮的 JSON 输出

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/how-to-enable-pretty-print-json-output-jackson/>

在 Jackson 中，我们可以使用`writerWithDefaultPrettyPrinter()`来打印 JSON 输出。

*用杰克逊 2.9.8 测试*

## 1.漂亮的打印 JSON

1.1 默认情况下，Jackson 以紧凑格式打印:

```java
 ObjectMapper mapper = new ObjectMapper();
	Staff staff = createStaff();
	String json = mapper.writeValueAsString(staff);
	System.out.println(json); 
```

输出

```java
 {"name":"mkyong","age":38,"skills":["java","python","node","kotlin"]} 
```

1.2 实现按需打印。

```java
 ObjectMapper mapper = new ObjectMapper();
	Staff staff = createStaff();
	// pretty print
	String json = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(staff);
	System.out.println(json); 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "skills" : [ "java", "python", "node", "kotlin" ]
} 
```

1.3 全球启用 pretty print。

```java
 import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

	// pretty print
	ObjectMapper mapper = new ObjectMapper().enable(SerializationFeature.INDENT_OUTPUT);
	Staff staff = createStaff();
	String json = mapper.writeValueAsString(staff);
	System.out.println(json); 
```

输出

```java
 {
  "name" : "mkyong",
  "age" : 38,
  "skills" : [ "java", "python", "node", "kotlin" ]
} 
```

**Note**
To display the pretty print JSON output on an HTML page, wraps it with `pre` tags.
`<pre>${pretty-print-json-output}</pre>`**Note – 12/12/2013**
The article is updated to use `writerWithDefaultPrettyPrinter()`, the old `defaultPrettyPrintingWriter()` is deprecated.

## 参考

*   [Jackson–如何解析 JSON](http://web.archive.org/web/20220803152933/https://www.mkyong.com/java/jackson-how-to-parse-json/)
*   [Gson–如何启用 pretty print JSON 输出](http://web.archive.org/web/20220803152933/https://www.mkyong.com/java/how-to-enable-pretty-print-json-output-gson/)

<input type="hidden" id="mkyong-current-postId" value="9947">