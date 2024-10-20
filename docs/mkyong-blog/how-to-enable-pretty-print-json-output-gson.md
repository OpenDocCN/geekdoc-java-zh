# gson——如何实现漂亮的 JSON 输出

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/java/how-to-enable-pretty-print-json-output-gson/>

在本教程中，我们将向您展示如何在 [Gson](http://web.archive.org/web/20221117184233/https://github.com/google/gson) 框架中启用 JSON pretty print。

1.默认情况下，Gson 压缩打印 JSON 输出:

GsonExample1.java

```java
 package com.mkyong;

import com.google.gson.Gson;

public class GsonExample1 {

    public static void main(String[] args) {

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

2.要启用 JSON 漂亮打印，用`GsonBuilder`创建`Gson`对象

GsonExample2.java

```java
 package com.mkyong;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class GsonExample2 {

    public static void main(String[] args) {

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

**Note**
Read more [Gson examples](http://web.archive.org/web/20221117184233/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

## 参考

*   [GsonBuilder Javadocs](http://web.archive.org/web/20221117184233/https://google.github.io/gson/apidocs/com/google/gson/GsonBuilder.html)
*   [Gson–如何解析 JSON 示例](http://web.archive.org/web/20221117184233/https://www.mkyong.com/java/how-to-parse-json-with-gson/)

<input type="hidden" id="mkyong-current-postId" value="9934">