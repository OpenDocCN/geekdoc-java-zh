# 杰克逊树模型示例

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/jackson-tree-model-example/>

在 Jackson 中，我们可以使用`Tree Model`来表示 JSON 结构，并通过`JsonNode`执行 CRUD 操作，类似于 XML DOM 树。这个杰克逊`Tree Model`非常有用，尤其是在 JSON 结构不能很好地映射到 Java 类的情况下。

pom.xml

```java
 <dependency>
		<groupId>com.fasterxml.jackson.core</groupId>
		<artifactId>jackson-databind</artifactId>
		<version>2.9.8</version>
	</dependency> 
```

*用杰克逊 2.9.8 测试的 PS*

## 1.遍历 JSON

1.1 Jackson `TreeModel`遍历以下 JSON 文件的示例:

C:\\projects\\user.json

```java
 {
  "id": 1,
  "name": {
    "first": "Yong",
    "last": "Mook Kim"
  },
  "contact": [
    {
      "type": "phone/home",
      "ref": "111-111-1234"
    },
    {
      "type": "phone/work",
      "ref": "222-222-2222"
    }
  ]
} 
```

1.2 逐个处理`JsonNode`。

JacksonTreeModelExample1.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonGenerationException;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.File;
import java.io.IOException;

public class JacksonTreeModelExample1 {

    private static final ObjectMapper mapper = new ObjectMapper();

    public static void main(String[] args) {

        try {

            JsonNode root = mapper.readTree(new File("c:\\projects\\user.json"));

            // Get id
            long id = root.path("id").asLong();
            System.out.println("id : " + id);

            // Get Name
            JsonNode nameNode = root.path("name");
            if (!nameNode.isMissingNode()) {        // if "name" node is exist
                System.out.println("firstName : " + nameNode.path("first").asText());
                System.out.println("middleName : " + nameNode.path("middle").asText());
                System.out.println("lastName : " + nameNode.path("last").asText());
            }

            // Get Contact
            JsonNode contactNode = root.path("contact");
            if (contactNode.isArray()) {

                System.out.println("Is this node an Array? " + contactNode.isArray());

                for (JsonNode node : contactNode) {
                    String type = node.path("type").asText();
                    String ref = node.path("ref").asText();
                    System.out.println("type : " + type);
                    System.out.println("ref : " + ref);

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
 id : 1

firstName : Yong
middleName : 
lastName : Mook Kim

Is this node an Array? true
type : phone/home
ref : 111-111-1234
type : phone/work
ref : 222-222-2222 
```

## 2.遍历 JSON 数组

2.1 JSON 文件，顶层代表一个数组。

c:\\projects\\user2.json

```java
 [
  {
    "id": 1,
    "name": {
      "first": "Yong",
      "last": "Mook Kim"
    },
    "contact": [
      {
        "type": "phone/home",
        "ref": "111-111-1234"
      },
      {
        "type": "phone/work",
        "ref": "222-222-2222"
      }
    ]
  },
  {
    "id": 2,
    "name": {
      "first": "Yong",
      "last": "Zi Lap"
    },
    "contact": [
      {
        "type": "phone/home",
        "ref": "333-333-1234"
      },
      {
        "type": "phone/work",
        "ref": "444-444-4444"
      }
    ]
  }
] 
```

2.2 概念是一样的，只是循环 JSON 数组:

```java
 JsonNode rootArray = mapper.readTree(new File("c:\\projects\\user2.json"));

	for (JsonNode root : rootArray) {
		// get node like the above example 1
	} 
```

JacksonTreeModelExample2.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonGenerationException;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.File;
import java.io.IOException;

public class JacksonTreeModelExample2 {

    private static final ObjectMapper mapper = new ObjectMapper();

    public static void main(String[] args) {

        try {

            JsonNode rootArray = mapper.readTree(new File("c:\\projects\\user2.json"));

            for (JsonNode root : rootArray) {

                // Get id
                long id = root.path("id").asLong();
                System.out.println("id : " + id);

                // Get Name
                JsonNode nameNode = root.path("name");
                if (!nameNode.isMissingNode()) {        // if "name" node is exist
                    System.out.println("firstName : " + nameNode.path("first").asText());
                    System.out.println("middleName : " + nameNode.path("middle").asText());
                    System.out.println("lastName : " + nameNode.path("last").asText());
                }

                // Get Contact
                JsonNode contactNode = root.path("contact");
                if (contactNode.isArray()) {

                    System.out.println("Is this node an Array? " + contactNode.isArray());

                    for (JsonNode node : contactNode) {
                        String type = node.path("type").asText();
                        String ref = node.path("ref").asText();
                        System.out.println("type : " + type);
                        System.out.println("ref : " + ref);

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
 id : 1
firstName : Yong
middleName : 
lastName : Mook Kim
Is this node an Array? true
type : phone/home
ref : 111-111-1234
type : phone/work
ref : 222-222-2222

id : 2
firstName : Yong
middleName : 
lastName : Zi Lap
Is this node an Array? true
type : phone/home
ref : 333-333-1234
type : phone/work
ref : 444-444-4444 
```

## 3.树形模型 CRUD 示例

3.1 这个例子，向你展示了如何创建、更新和删除 JSON 节点，要修改 JSON 节点，我们需要将其转换为`ObjectNode`。阅读评论，不言自明。

JacksonTreeModelExample3.java

```java
 package com.mkyong;

import com.fasterxml.jackson.core.JsonGenerationException;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.node.ArrayNode;
import com.fasterxml.jackson.databind.node.ObjectNode;

import java.io.File;
import java.io.IOException;

public class JacksonTreeModelExample3 {

    private static final ObjectMapper mapper = new ObjectMapper();

    public static void main(String[] args) {

        try {

            JsonNode root = mapper.readTree(new File("c:\\projects\\user.json"));

            String resultOriginal = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(root);
            System.out.println("Before Update " + resultOriginal);

            // 1\. Update id to 1000
            ((ObjectNode) root).put("id", 1000L);

            // 2\. If middle name is empty , update to M
            ObjectNode nameNode = (ObjectNode) root.path("name");
            if ("".equals(nameNode.path("middle").asText())) {
                nameNode.put("middle", "M");
            }

            // 3\. Create a new field in nameNode
            nameNode.put("nickname", "mkyong");

            // 4\. Remove last field in nameNode
            nameNode.remove("last");

            // 5\. Create a new ObjectNode and add to root
            ObjectNode positionNode = mapper.createObjectNode();
            positionNode.put("name", "Developer");
            positionNode.put("years", 10);
            ((ObjectNode) root).set("position", positionNode);

            // 6\. Create a new ArrayNode and add to root
            ArrayNode gamesNode = mapper.createArrayNode();

            ObjectNode game1 = mapper.createObjectNode().objectNode();
            game1.put("name", "Fall Out 4");
            game1.put("price", 49.9);

            ObjectNode game2 = mapper.createObjectNode().objectNode();
            game2.put("name", "Dark Soul 3");
            game2.put("price", 59.9);

            gamesNode.add(game1);
            gamesNode.add(game2);
            ((ObjectNode) root).set("games", gamesNode);

            // 7\. Append a new Node to ArrayNode
            ObjectNode email = mapper.createObjectNode();
            email.put("type", "email");
            email.put("ref", "abc@mkyong.com");

            JsonNode contactNode = root.path("contact");
            ((ArrayNode) contactNode).add(email);

            String resultUpdate = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(root);

            System.out.println("After Update " + resultUpdate);

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
 Before Update {
  "id" : 1,
  "name" : {
    "first" : "Yong",
    "last" : "Mook Kim"
  },
  "contact" : [ {
    "type" : "phone/home",
    "ref" : "111-111-1234"
  }, {
    "type" : "phone/work",
    "ref" : "222-222-2222"
  } ]
}

After Update {
  "id" : 1000,
  "name" : {
    "first" : "Yong",
    "middle" : "M",
    "nickname" : "mkyong"
  },
  "contact" : [ {
    "type" : "phone/home",
    "ref" : "111-111-1234"
  }, {
    "type" : "phone/work",
    "ref" : "222-222-2222"
  }, {
    "type" : "email",
    "ref" : "abc@mkyong.com"
  } ],
  "position" : {
    "name" : "Developer",
    "years" : 10
  },
  "games" : [ {
    "name" : "Fall Out 4",
    "price" : 49.9
  }, {
    "name" : "Dark Soul 3",
    "price" : 59.9
  } ]
} 
```

## 参考

*   [使用 Jackson 的 Json 处理:方法#3/3:树遍历](http://web.archive.org/web/20211024095240/http://www.cowtowncoder.com/blog/archives/2009/01/entry_153.html)
*   [杰克逊数据绑定指南](http://web.archive.org/web/20211024095240/https://github.com/FasterXML/jackson-databind/)
*   [Jackson 2–将 Java 对象转换成 JSON 从 JSON 转换过来](http://web.archive.org/web/20211024095240/https://www.mkyong.com/java/jackson-2-convert-java-object-to-from-json/)
*   [stack overflow——使用 Jackson 向 JSON 添加属性](http://web.archive.org/web/20211024095240/https://stackoverflow.com/questions/23271699/adding-property-to-json-using-jackson)

<input type="hidden" id="mkyong-current-postId" value="9955">