# JSON . simple——读写 JSON

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/json-simple-example-read-and-write-json/>

[JSON.simple](http://web.archive.org/web/20221117050521/https://github.com/fangyidong/json-simple) 是一个简单的 Java 库，用于 JSON 处理、读写 JSON 数据，完全符合 [JSON 规范(RFC4627)](http://web.archive.org/web/20221117050521/https://www.ietf.org/rfc/rfc4627.txt)

**Warning**
This article is using the old `JSON.simple 1.x` ,which is deprecated and no longer maintained by the author. Please visit this upgraded article – [JSON.simple 3.x – How to parse JSON](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/json-simple-how-to-parse-json/)**Why not Jackson or Gson?**
You may have interest to read this article – How to parse JSON with [Jackson](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-how-to-parse-json/) or [Gson](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

## 1.下载 JSON.simple

pom.xml

```java
 <dependency>
		<groupId>com.googlecode.json-simple</groupId>
		<artifactId>json-simple</artifactId>
		<version>1.1.1</version>
	</dependency> 
```

## 2.将 JSON 写入文件

JsonSimpleWriteExample.java

```java
 package com.mkyong;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

import java.io.FileWriter;
import java.io.IOException;

public class JsonSimpleWriteExample {

    public static void main(String[] args) {

        JSONObject obj = new JSONObject();
        obj.put("name", "mkyong.com");
        obj.put("age", 100);

        JSONArray list = new JSONArray();
        list.add("msg 1");
        list.add("msg 2");
        list.add("msg 3");

        obj.put("messages", list);

        try (FileWriter file = new FileWriter("c:\\projects\\test.json")) {
            file.write(obj.toJSONString());
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.print(obj);

    }

} 
```

输出

Terminal

```java
 {"name":"mkyong.com","messages":["msg 1","msg 2","msg 3"],"age":100} 
```

c:\\projects\\test.json

```java
 {"name":"mkyong.com","messages":["msg 1","msg 2","msg 3"],"age":100} 
```

## 3.将 JSON 读取到文件

JsonSimpleReadExample.java

```java
 package com.mkyong;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;
import java.util.Iterator;

public class JsonSimpleReadExample {

    public static void main(String[] args) {

        JSONParser parser = new JSONParser();

        try (Reader reader = new FileReader("c:\\projects\\test.json")) {

            JSONObject jsonObject = (JSONObject) parser.parse(reader);
            System.out.println(jsonObject);

            String name = (String) jsonObject.get("name");
            System.out.println(name);

            long age = (Long) jsonObject.get("age");
            System.out.println(age);

            // loop array
            JSONArray msg = (JSONArray) jsonObject.get("messages");
            Iterator<String> iterator = msg.iterator();
            while (iterator.hasNext()) {
                System.out.println(iterator.next());
            }

        } catch (IOException e) {
            e.printStackTrace();
        } catch (ParseException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 {"name":"mkyong.com","messages":["msg 1","msg 2","msg 3"],"age":100}
mkyong.com
100
msg 1
msg 2
msg 3 
```

## 参考

*   [JSON.simple 3.x 首页](http://web.archive.org/web/20221117050521/https://cliftonlabs.github.io/json-simple/)
*   [JSON.simple 1.x 首页](http://web.archive.org/web/20221117050521/https://github.com/fangyidong/json-simple)
*   [JSON.simple 1.x 谷歌主页](http://web.archive.org/web/20221117050521/https://code.google.com/archive/p/json-simple/)
*   [JSON.simple 1.x 编码示例](http://web.archive.org/web/20221117050521/https://code.google.com/archive/p/json-simple/wikis/EncodingExamples.wiki)
*   [JSON . simple 1 . x decoding 示例](http://web.archive.org/web/20221117050521/https://code.google.com/archive/p/json-simple/wikis/DecodingExamples.wiki)
*   [Gson–如何处理 JSON](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/how-to-parse-json-with-gson/)
*   [Jackson–如何解析 JSON](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/jackson-how-to-parse-json/)
*   [JSON . simple 3 . x–如何解析 JSON](http://web.archive.org/web/20221117050521/https://www.mkyong.com/java/json-simple-how-to-parse-json/)

<input type="hidden" id="mkyong-current-postId" value="9975">