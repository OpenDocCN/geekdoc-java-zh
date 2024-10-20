# Java——如何从 Map 中获取键和值

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-how-to-get-keys-and-values-from-map/>

在 Java 中，我们可以通过`map.entrySet()`获得键和值

```java
 Map<String, String> map = new HashMap<>();

	// Get keys and values
	for (Map.Entry<String, String> entry : map.entrySet()) {
		String k = entry.getKey();
		String v = entry.getValue();
		System.out.println("Key: " + k + ", Value: " + v);
	}

	// Java 8
	map.forEach((k, v) -> {
		System.out.println("Key: " + k + ", Value: " + v);
	}); 
```

完整的例子。

JavaMapExample.java

```java
 package com.mkyong;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class JavaMapExample {

    public static void main(String[] args) {

        Map<String, String> map = new HashMap<>();
        map.put("db", "oracle");
        map.put("username", "user1");
        map.put("password", "pass1");

        // Get keys and values
        for (Map.Entry<String, String> entry : map.entrySet()) {
            String k = entry.getKey();
            String v = entry.getValue();
            System.out.println("Key: " + k + ", Value: " + v);
        }

        // Get all keys
        Set<String> keys = map.keySet();
        for (String k : keys) {
            System.out.println("Key: " + k);
        }

        // Get all values
        Collection<String> values = map.values();
        for (String v : values) {
            System.out.println("Value: " + v);
        }

        // Java 8
        map.forEach((k, v) -> {
            System.out.println("Key: " + k + ", Value: " + v);
        });

    }
} 
```

输出

```java
 Key: password, Value: pass1
Key: db, Value: oracle
Key: username, Value: user1

Key: password
Key: db
Key: username

Value: pass1
Value: oracle
Value: user1

Key: password, Value: pass1
Key: db, Value: oracle
Key: username, Value: user1 
```

## 参考

*   [如何在 Java 中循环地图](http://web.archive.org/web/20221205155212/https://www.mkyong.com/java/how-to-loop-a-map-in-java/)
*   [Java 散列表示例](http://web.archive.org/web/20221205155212/https://www.mkyong.com/java/how-to-use-hashmap-tutorial-java/)

<input type="hidden" id="mkyong-current-postId" value="15160">