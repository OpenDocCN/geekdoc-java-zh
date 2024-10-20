# Java MongoDB:插入文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-mongodb-insert-a-document/>

在本教程中，我们向您展示了通过 Java MongoDB API 将下面的 **JSON** 数据插入到一个**文档**中的 4 种方法。

## 测试数据

JSON 格式的测试数据。

```java
 {
	"database" : "mkyongDB",
	"table" : "hosting",
	"detail" : 
		{
			records : 99,
			index : "vps_index1",
			active : "true"
		}
	}
} 
```

 ## 1.BasicDBObject 示例

```java
 BasicDBObject document = new BasicDBObject();
	document.put("database", "mkyongDB");
	document.put("table", "hosting");

	BasicDBObject documentDetail = new BasicDBObject();
	documentDetail.put("records", 99);
	documentDetail.put("index", "vps_index1");
	documentDetail.put("active", "true");

	document.put("detail", documentDetail);

	collection.insert(document); 
```

 ## 2.BasicDBObjectBuilder 示例

```java
 BasicDBObjectBuilder documentBuilder = BasicDBObjectBuilder.start()
		.add("database", "mkyongDB")
		.add("table", "hosting");

	BasicDBObjectBuilder documentBuilderDetail = BasicDBObjectBuilder.start()
	.add("records", 99)
	.add("index", "vps_index1")
	.add("active", "true");

	documentBuilder.add("detail", documentBuilderDetail.get());

	collection.insert(documentBuilder.get()); 
```

## 3.地图示例

```java
 Map<String, Object> documentMap = new HashMap<String, Object>();
	documentMap.put("database", "mkyongDB");
	documentMap.put("table", "hosting");

	Map<String, Object> documentMapDetail = new HashMap<String, Object>();
	documentMapDetail.put("records", 99);
	documentMapDetail.put("index", "vps_index1");
	documentMapDetail.put("active", "true");

	documentMap.put("detail", documentMapDetail);

	collection.insert(new BasicDBObject(documentMap)); 
```

## 4.JSON 解析示例

```java
 String json = "{'database' : 'mkyongDB','table' : 'hosting'," +
	  "'detail' : {'records' : 99, 'index' : 'vps_index1', 'active' : 'true'}}}";

	DBObject dbObject = (DBObject)JSON.parse(json);

	collection.insert(dbObject); 
```

## 完整示例

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import java.util.HashMap;
import java.util.Map;
import com.mongodb.BasicDBObject;
import com.mongodb.BasicDBObjectBuilder;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.DBObject;
import com.mongodb.Mongo;
import com.mongodb.MongoException;
import com.mongodb.util.JSON;

/**
 * Java MongoDB : Insert a Document
 * 
 */
public class InsertDocumentApp {
  public static void main(String[] args) {

    try {

	Mongo mongo = new Mongo("localhost", 27017);
	DB db = mongo.getDB("yourdb");

	DBCollection collection = db.getCollection("dummyColl");

	// 1\. BasicDBObject example
	System.out.println("BasicDBObject example...");
	BasicDBObject document = new BasicDBObject();
	document.put("database", "mkyongDB");
	document.put("table", "hosting");

	BasicDBObject documentDetail = new BasicDBObject();
	documentDetail.put("records", 99);
	documentDetail.put("index", "vps_index1");
	documentDetail.put("active", "true");
	document.put("detail", documentDetail);

	collection.insert(document);

	DBCursor cursorDoc = collection.find();
	while (cursorDoc.hasNext()) {
		System.out.println(cursorDoc.next());
	}

	collection.remove(new BasicDBObject());

	// 2\. BasicDBObjectBuilder example
	System.out.println("BasicDBObjectBuilder example...");
	BasicDBObjectBuilder documentBuilder = BasicDBObjectBuilder.start()
		.add("database", "mkyongDB")
                .add("table", "hosting");

	BasicDBObjectBuilder documentBuilderDetail = BasicDBObjectBuilder.start()
                .add("records", "99")
                .add("index", "vps_index1")
		.add("active", "true");

	documentBuilder.add("detail", documentBuilderDetail.get());

	collection.insert(documentBuilder.get());

	DBCursor cursorDocBuilder = collection.find();
	while (cursorDocBuilder.hasNext()) {
		System.out.println(cursorDocBuilder.next());
	}

	collection.remove(new BasicDBObject());

	// 3\. Map example
	System.out.println("Map example...");
	Map<String, Object> documentMap = new HashMap<String, Object>();
	documentMap.put("database", "mkyongDB");
	documentMap.put("table", "hosting");

	Map<String, Object> documentMapDetail = new HashMap<String, Object>();
	documentMapDetail.put("records", "99");
	documentMapDetail.put("index", "vps_index1");
	documentMapDetail.put("active", "true");

	documentMap.put("detail", documentMapDetail);

	collection.insert(new BasicDBObject(documentMap));

	DBCursor cursorDocMap = collection.find();
	while (cursorDocMap.hasNext()) {
		System.out.println(cursorDocMap.next());
	}

	collection.remove(new BasicDBObject());

	// 4\. JSON parse example
	System.out.println("JSON parse example...");

	String json = "{'database' : 'mkyongDB','table' : 'hosting'," +
	  "'detail' : {'records' : 99, 'index' : 'vps_index1', 'active' : 'true'}}}";

	DBObject dbObject = (DBObject)JSON.parse(json);

	collection.insert(dbObject);

	DBCursor cursorDocJSON = collection.find();
	while (cursorDocJSON.hasNext()) {
		System.out.println(cursorDocJSON.next());
	}

	collection.remove(new BasicDBObject());

    } catch (UnknownHostException e) {
	e.printStackTrace();
    } catch (MongoException e) {
	e.printStackTrace();
    }

  }
} 
```

*输出…*

```java
 BasicDBObject example...
{ "_id" : { "$oid" : "4dc9ef6f237f86642d5b34bd"} , "database" : "mkyongDB" , 
"table" : "hosting" , "detail" : { "records" : "99" , "index" : "vps_index1" , "active" : "true"}}

BasicDBObjectBuilder example...
{ "_id" : { "$oid" : "4dc9ef6f237f86642d5b34be"} , "database" : "mkyongDB" , 
"table" : "hosting" , "detail" : { "records" : "99" , "index" : "vps_index1" , "active" : "true"}}

Map example...
{ "_id" : { "$oid" : "4dc9ef6f237f86642d5b34bf"} , "detail" : { "index" : "vps_index1" , 
"active" : "true" , "records" : "99"} , "table" : "hosting" , "database" : "mkyongDB"}

JSON parse example...
{ "_id" : { "$oid" : "4dc9ef6f237f86642d5b34c0"} , "database" : "mkyongDB" , 
"table" : "hosting" , "detail" : { "records" : 199 , "index" : "vps_index1" , "active" : "true"}} 
```

**What is “_id” ?**
The `_id` is added by MongoDB automatically, for identity purpose. From MongoDB document, it said, all element names that start with “_”, “/” and “$” are reserved for internal use.

## 参考

1.  [基础对象 Java 文档](http://web.archive.org/web/20190215001447/http://api.mongodb.org/java/2.10.1/com/mongodb/BasicDBObject.html)
2.  [基础对象构建器 Java 文档](http://web.archive.org/web/20190215001447/http://api.mongodb.org/java/2.10.1/com/mongodb/BasicDBObjectBuilder.html)

[mongodb](http://web.archive.org/web/20190215001447/http://www.mkyong.com/tag/mongodb/) [save](http://web.archive.org/web/20190215001447/http://www.mkyong.com/tag/save/)







