# gson——以流的形式读写 JSON

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/gson-streaming-to-read-and-write-json/>

从 Gson 版本 1.6 开始，引入了两个新的类`JsonReader`和`JsonWriter`来提供对 JSON 数据的流处理。阅读此 [Gson 流媒体文档](http://web.archive.org/web/20211024032553/https://sites.google.com/site/gson/streaming)以了解使用它的好处。

*   `JsonWriter`–将 JSON 写成一个流。
*   `JsonReader`–以流的形式读取 JSON。

## 1.JsonWriter

GsonExample1.java

```java
 package com.mkyong;

import com.google.gson.stream.JsonWriter;

import java.io.FileWriter;
import java.io.IOException;

public class GsonExample1 {

    public static void main(String[] args) {

        try (JsonWriter writer = new JsonWriter(new FileWriter("c:\\projects\\user.json"))) {

            writer.beginObject();                   // {
            writer.name("name").value("mkyong");    // "name" : "mkyong"
            writer.name("age").value(29);           // "age" : 29

            writer.name("messages");                // "messages" :
            writer.beginArray();                    // [
            writer.value("msg 1");                  // "msg 1"
            writer.value("msg 2");                  // "msg 2"
            writer.value("msg 3");                  // "msg 3"
            writer.endArray();                      // ]

            writer.endObject();                     // }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

c:\\projects\\user.json

```java
 {"name":"mkyong","age":29,"messages":["msg 1","msg 2","msg 3"]} 
```

## 2.JsonReader

**令牌**在流模式下，每一个 JSON 数据都被认为是一个单独的令牌。当你使用`JsonReader`来处理它的时候，每个令牌都会被顺序处理。举个例子，

```java
 {
	"url":"www.mkyong.com"
} 
```

*   令牌 1 = {
*   令牌 2 = url
*   令牌 3 = www.mkyong.com
*   令牌 4 = }

GsonExample2.java

```java
 package com.mkyong;

import com.google.gson.stream.JsonReader;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class GsonExample2 {

    public static void main(String[] args) {

        try (JsonReader reader = new JsonReader(new FileReader("c:\\projects\\user.json"))) {

            reader.beginObject();

            while (reader.hasNext()) {

                String name = reader.nextName();

                if (name.equals("name")) {

                    System.out.println(reader.nextString());

                } else if (name.equals("age")) {

                    System.out.println(reader.nextInt());

                } else if (name.equals("messages")) {

                    // read array
                    reader.beginArray();

                    while (reader.hasNext()) {
                        System.out.println(reader.nextString());
                    }

                    reader.endArray();

                } else {
                    reader.skipValue(); //avoid some unhandle events
                }
            }

            reader.endObject();

        } catch (FileNotFoundException e) {
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
29
msg 1
msg 2
msg 3 
```

**Note**
Read more [Gson examples](http://web.archive.org/web/20211024032553/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

## 参考

*   [Gson 流媒体示例](http://web.archive.org/web/20211024032553/https://sites.google.com/site/gson/streaming)
*   [杰克逊流媒体示例](http://web.archive.org/web/20211024032553/https://www.mkyong.com/java/jackson-streaming-api-to-read-and-write-json/)
*   [Gson–如何解析 JSON](http://web.archive.org/web/20211024032553/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

<input type="hidden" id="mkyong-current-postId" value="9969">