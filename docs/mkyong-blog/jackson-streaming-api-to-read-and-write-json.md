# Jackson–流模型示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/java-json-tutorials/url=https://www.mkyong.com/java/jackson-streaming-api-to-read-and-write-json/>

这个 Jackson 教程向你展示了如何使用`JsonGenerator`将 JSON 字符串和 JSON 数组写入一个文件，并使用`JsonParser`读取它

杰克逊流式 API

*   `JsonGenerator`–编写 JSON
*   `JsonParser` –解析 JSON

**Note**
The Jackson streaming mode is the underlying processing model that [data-binding](http://web.archive.org/web/20221205181757/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/) and [Tree Model](http://web.archive.org/web/20221205181757/https://www.mkyong.com/java/jackson-tree-model-example/) both build upon. It is the best performance and control over the JSON parsing and JSON generation.

*用杰克逊 2.9.8 测试*

## 1.JSON generator–编写 JSON

1.1 将 JSON 写入文件。

JacksonExample1.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonEncoding;
import com.fasterxml.jackson.core.JsonGenerationException;
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.File;
import java.io.IOException;

public class JacksonExample1 {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        try (JsonGenerator jGenerator =
                     mapper.getFactory().createGenerator(
                             new File("c:\\projects\\user.json")
                             , JsonEncoding.UTF8)) {

            jGenerator.writeStartObject();                                  // {

            jGenerator.writeStringField("name", "mkyong");  				// "name" : "mkyong"
            jGenerator.writeNumberField("age", 38);         				// "age" : 38

            jGenerator.writeFieldName("messages");                          // "messages" :

            jGenerator.writeStartArray();                                   // [

            jGenerator.writeString("msg 1");                            		// "msg 1"
            jGenerator.writeString("msg 2");                            		// "msg 2"
            jGenerator.writeString("msg 3");                            		// "msg 3"

            jGenerator.writeEndArray();                                     // ]

            jGenerator.writeEndObject();                                    // }

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

c:\\projects\\user.json

```java
 {"name":"mkyong","age":38,"messages":["msg 1","msg 2","msg 3"]} 
```

## 2.JSON generator–编写 JSON 数组

2.1 1.1 将 JSON 数组写入文件。

JacksonExample2.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonEncoding;
import com.fasterxml.jackson.core.JsonGenerationException;
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.File;
import java.io.IOException;

public class JacksonExample2 {

    public static void main(String[] args) {

        ObjectMapper mapper = new ObjectMapper();

        try (JsonGenerator jGenerator =
                     mapper.getFactory().createGenerator(
                             new File("c:\\projects\\user2.json")
                             , JsonEncoding.UTF8)) {

            // pretty print
            jGenerator.useDefaultPrettyPrinter();

            // start array
            jGenerator.writeStartArray();                                   // [

            jGenerator.writeStartObject();                                  // {

            jGenerator.writeStringField("name", "mkyong");  				// "name" : "mkyong"
            jGenerator.writeNumberField("age", 38);         				// "age" : 38

            jGenerator.writeFieldName("messages");                          // "messages" :

            jGenerator.writeStartArray();                                   // [

            jGenerator.writeString("msg 1");                            	// "msg 1"
            jGenerator.writeString("msg 2");                            	// "msg 2"
            jGenerator.writeString("msg 3");                            	// "msg 3"

            jGenerator.writeEndArray();                                     // ]

            jGenerator.writeEndObject();                                    // }

            // next object, pls

            jGenerator.writeStartObject();                                  // {

            jGenerator.writeStringField("name", "lap");  					// "name" : "lap"
            jGenerator.writeNumberField("age", 5);         					// "age" : 5

            jGenerator.writeFieldName("messages");                          // "messages" :

            jGenerator.writeStartArray();                                   // [

            jGenerator.writeString("msg a");                            	// "msg a"
            jGenerator.writeString("msg b");                            	// "msg b"
            jGenerator.writeString("msg c");                            	// "msg c"

            jGenerator.writeEndArray();                                     // ]

            jGenerator.writeEndObject();                                    // }

            jGenerator.writeEndArray();                                     // ]

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

c:\\projects\\user2.json

```java
 [ 
	{
	  "name" : "mkyong",
	  "age" : 38,
	  "messages" : [ "msg 1", "msg 2", "msg 3" ]
	}, {
	  "name" : "lap",
	  "age" : 5,
	  "messages" : [ "msg a", "msg b", "msg c" ]
	} 
] 
```

## 3.JSON parser–读取 JSON

**令牌**
在 Jackson streaming 模式下，它将 JSON 字符串拆分成一个令牌列表，每个令牌都会被增量处理。举个例子，

```java
 {
   "name":"mkyong"
} 
```

*   令牌 1 = {
*   令牌 2 =名称
*   令牌 3 = mkyong
*   令牌 4 = }

3.1 `JsonParser`解析 JSON 文件的示例。

c:\\projects\\user.json

```java
 {"name":"mkyong","age":38,"messages":["msg 1","msg 2","msg 3"]} 
```

JacksonExample3.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonFactory;
import com.fasterxml.jackson.core.JsonGenerationException;
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.core.JsonToken;
import com.fasterxml.jackson.databind.JsonMappingException;

import java.io.File;
import java.io.IOException;

public class JacksonExample3 {

    public static void main(String[] args) {

        try (JsonParser jParser = new JsonFactory()
				.createParser(new File("c:\\projects\\user.json"));) {

            // loop until token equal to "}"
            while (jParser.nextToken() != JsonToken.END_OBJECT) {

                String fieldname = jParser.getCurrentName();

                if ("name".equals(fieldname)) {
                    // current token is "name",
                    // move to next, which is "name"'s value
                    jParser.nextToken();
                    System.out.println(jParser.getText());
                }

                if ("age".equals(fieldname)) {
                    jParser.nextToken();
                    System.out.println(jParser.getIntValue());
                }

                if ("messages".equals(fieldname)) {

                    if (jParser.nextToken() == JsonToken.START_ARRAY) {
                        // messages is array, loop until token equal to "]"
                        while (jParser.nextToken() != JsonToken.END_ARRAY) {
                            System.out.println(jParser.getText());
                        }
                    }

                }

            }

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
 mkyong
38
msg 1
msg 2
msg 3 
```

## 4.JSON parser–读取 JSON 数组

4.1 `JsonParser`解析 JSON 数组文件的例子。

c:\\projects\\user2.json

```java
 [ 
	{
	  "name" : "mkyong",
	  "age" : 38,
	  "messages" : [ "msg 1", "msg 2", "msg 3" ]
	}, {
	  "name" : "lap",
	  "age" : 5,
	  "messages" : [ "msg a", "msg b", "msg c" ]
	} 
] 
```

JacksonExample4.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonFactory;
import com.fasterxml.jackson.core.JsonGenerationException;
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.core.JsonToken;
import com.fasterxml.jackson.databind.JsonMappingException;

import java.io.File;
import java.io.IOException;

public class JacksonExample4 {

    public static void main(String[] args) {

        try (JsonParser jParser = new JsonFactory()
                .createParser(new File("c:\\projects\\user2.json"));) {

            // JSON array?
            if (jParser.nextToken() == JsonToken.START_ARRAY) {

                while (jParser.nextToken() != JsonToken.END_ARRAY) {

                    // loop until token equal to "}"
                    while (jParser.nextToken() != JsonToken.END_OBJECT) {

                        String fieldname = jParser.getCurrentName();
                        if ("name".equals(fieldname)) {
                            // current token is "name",
                            // move to next, which is "name"'s value
                            jParser.nextToken();
                            System.out.println(jParser.getText());
                        }

                        if ("age".equals(fieldname)) {
                            jParser.nextToken();
                            System.out.println(jParser.getIntValue());
                        }

                        if ("messages".equals(fieldname)) {

                            //jParser.nextToken(); // current token is "[", move next
                            if (jParser.nextToken() == JsonToken.START_ARRAY) {
                                // messages is array, loop until token equal to "]"
                                while (jParser.nextToken() != JsonToken.END_ARRAY) {
                                    System.out.println(jParser.getText());
                                }
                            }

                        }

                    }

                }
            }

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
 mkyong
38
msg 1
msg 2
msg 3
lap
5
msg a
msg b
msg c 
```

**Note**
More [Jackson examples](http://web.archive.org/web/20221205181757/https://www.mkyong.com/java/jackson-how-to-parse-json/)

## 参考

*   [杰克逊流媒体应用编程接口](http://web.archive.org/web/20221205181757/https://github.com/FasterXML/jackson-docs/wiki/JacksonStreamingApi)
*   [杰克逊数据绑定](http://web.archive.org/web/20221205181757/https://github.com/FasterXML/jackson-databind/)
*   [Jackson–如何解析 JSON](http://web.archive.org/web/20221205181757/https://www.mkyong.com/java/jackson-how-to-parse-json/)
*   [杰克逊树模型示例](http://web.archive.org/web/20221205181757/https://www.mkyong.com/java/jackson-tree-model-example/)
*   [Jackson–将 Java 对象转换成 JSON 从 JSON 转换过来](http://web.archive.org/web/20221205181757/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/)
*   [Gson–如何处理 JSON](http://web.archive.org/web/20221205181757/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

<input type="hidden" id="mkyong-current-postId" value="9962">