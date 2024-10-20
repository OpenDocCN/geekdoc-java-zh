# Java MongoDB:将 JSON 数据转换为 DBObject

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-mongodb-convert-json-data-to-dbobject/>

MongoDB 附带了“ **com.mongodb.util.JSON** ”类，将 JSON 数据直接转换为 DBObject。例如，数据以 JSON 格式表示:

```java
 {
	'name' : 'mkyong',
	'age' : 30
} 
```

要将其转换为 DBObject，可以编写如下代码:

```java
 DBObject dbObject = (DBObject) JSON.parse("{'name':'mkyong', 'age':30}"); 
```

##### 例子

查看完整的示例，将上述 JSON 数据转换为 DBObject，并保存到 MongoDB 中。

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.DBObject;
import com.mongodb.Mongo;
import com.mongodb.MongoException;
import com.mongodb.util.JSON;

/**
 * Java MongoDB : Convert JSON data to DBObject
 * 
 */

public class App {
	public static void main(String[] args) {

		try {

			Mongo mongo = new Mongo("localhost", 27017);
			DB db = mongo.getDB("yourdb");
			DBCollection collection = db.getCollection("dummyColl");

			// convert JSON to DBObject directly
			DBObject dbObject = (DBObject) JSON
					.parse("{'name':'mkyong', 'age':30}");

			collection.insert(dbObject);

			DBCursor cursorDoc = collection.find();
			while (cursorDoc.hasNext()) {
				System.out.println(cursorDoc.next());
			}

			System.out.println("Done");

		} catch (UnknownHostException e) {
			e.printStackTrace();
		} catch (MongoException e) {
			e.printStackTrace();
		}
	}
} 
```

输出

```java
 { "_id" : { "$oid" : "4dc9ebb5237f275c2fe4959f"} , "name" : "mkyong" , "age" : 30}
Done 
```

Tags : [convert](http://web.archive.org/web/20210506225142/https://mkyong.com/tag/convert/) [json](http://web.archive.org/web/20210506225142/https://mkyong.com/tag/json/) [mongodb](http://web.archive.org/web/20210506225142/https://mkyong.com/tag/mongodb/)<input type="hidden" id="mkyong-current-postId" value="8854">