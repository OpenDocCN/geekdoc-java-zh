# Java MongoDB:查询文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-mongodb-query-document/>

在本教程中，我们将向您展示从集合中获取或查询文档的几种常用方法。

## 测试数据

插入 5 份模拟文件进行测试。

```java
 { "_id" : { "$oid" : "id"} , "number" : 1 , "name" : "mkyong-1"}
{ "_id" : { "$oid" : "id"} , "number" : 2 , "name" : "mkyong-2"}
{ "_id" : { "$oid" : "id"} , "number" : 3 , "name" : "mkyong-3"}
{ "_id" : { "$oid" : "id"} , "number" : 4 , "name" : "mkyong-4"}
{ "_id" : { "$oid" : "id"} , "number" : 5 , "name" : "mkyong-5"} 
```

 ## 1.查找()示例

1.1 仅获取第一个匹配的文档。

```java
 DBObject doc = collection.findOne();
	System.out.println(dbObject); 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "number" : 1 , "name" : "mkyong-1"} 
```

1.2 获取所有匹配的文档。

```java
 DBCursor cursor = collection.find();
	while(cursor.hasNext()) {
	    System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "number" : 1 , "name" : "mkyong-1"}
{ "_id" : { "$oid" : "id"} , "number" : 2 , "name" : "mkyong-2"}
{ "_id" : { "$oid" : "id"} , "number" : 3 , "name" : "mkyong-3"}
{ "_id" : { "$oid" : "id"} , "number" : 4 , "name" : "mkyong-4"}
{ "_id" : { "$oid" : "id"} , "number" : 5 , "name" : "mkyong-5"} 
```

1.3 从匹配的文档中获取单个字段。

```java
 BasicDBObject allQuery = new BasicDBObject();
	BasicDBObject fields = new BasicDBObject();
	fields.put("name", 1);

	DBCursor cursor = collection.find(allQuery, fields);
	while (cursor.hasNext()) {
		System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "name" : "mkyong-1"}
{ "_id" : { "$oid" : "id"} , "name" : "mkyong-2"}
{ "_id" : { "$oid" : "id"} , "name" : "mkyong-3"}
{ "_id" : { "$oid" : "id"} , "name" : "mkyong-4"}
{ "_id" : { "$oid" : "id"} , "name" : "mkyong-5"} 
```

 ## 2.查找()和比较

2.1 在`number = 5`处获取所有文件。

```java
 BasicDBObject whereQuery = new BasicDBObject();
	whereQuery.put("number", 5);
	DBCursor cursor = collection.find(whereQuery);
	while(cursor.hasNext()) {
	    System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "number" : 5 , "name" : "mkyong-5"} 
```

2.2 `$in`示例–在`number in 2, 4 and 5`处获取文件。

```java
 BasicDBObject inQuery = new BasicDBObject();
	List<Integer> list = new ArrayList<Integer>();
	list.add(2);
	list.add(4);
	list.add(5);
	inQuery.put("number", new BasicDBObject("$in", list));
	DBCursor cursor = collection.find(inQuery);
	while(cursor.hasNext()) {
		System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "number" : 2 , "name" : "mkyong-2"}
{ "_id" : { "$oid" : "id"} , "number" : 4 , "name" : "mkyong-4"}
{ "_id" : { "$oid" : "id"} , "number" : 5 , "name" : "mkyong-5"} 
```

2.3 `$gt $lt`示例–在 `5 > number > 2` 处获取文件。

```java
 BasicDBObject gtQuery = new BasicDBObject();
	gtQuery.put("number", new BasicDBObject("$gt", 2).append("$lt", 5));
	DBCursor cursor = collection.find(gtQuery);
	while(cursor.hasNext()) {
		System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "number" : 3 , "name" : "mkyong-3"}
{ "_id" : { "$oid" : "id"} , "number" : 4 , "name" : "mkyong-4"} 
```

2.4 `$ne`示例–在 `number != 4` 处获取文件。

```java
 BasicDBObject neQuery = new BasicDBObject();
	neQuery.put("number", new BasicDBObject("$ne", 4));
	DBCursor cursor = collection.find(neQuery);
	while(cursor.hasNext()) {
		System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "number" : 1 , "name" : "mkyong-1"}
{ "_id" : { "$oid" : "id"} , "number" : 2 , "name" : "mkyong-2"}
{ "_id" : { "$oid" : "id"} , "number" : 3 , "name" : "mkyong-3"}
{ "_id" : { "$oid" : "id"} , "number" : 5 , "name" : "mkyong-5"} 
```

## 3.find()和逻辑

3.1 `$and`示例-从`number = 2 and name = 'mkyong-2'`处获取文件。

```java
 BasicDBObject andQuery = new BasicDBObject();
	List<BasicDBObject> obj = new ArrayList<BasicDBObject>();
	obj.add(new BasicDBObject("number", 2));
	obj.add(new BasicDBObject("name", "mkyong-2"));
	andQuery.put("$and", obj);

	System.out.println(andQuery.toString());

	DBCursor cursor = collection.find(andQuery);
	while (cursor.hasNext()) {
		System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "$and" : [ { "number" : 2} , { "name" : "mkyong-2"}]}

{ "_id" : { "$oid" : "id"} , "number" : 2 , "name" : "mkyong-2"} 
```

## 4.find()和 Regex

使用正则表达式模式查找文档。

4.1 `$regex`示例-从`name like pattern 'Mky.*-[1-3]', case insensitive`处获取文件。

```java
 BasicDBObject regexQuery = new BasicDBObject();
	regexQuery.put("name", 
		new BasicDBObject("$regex", "Mky.*-[1-3]")
		.append("$options", "i"));

	System.out.println(regexQuery.toString());

	DBCursor cursor = collection.find(regexQuery);
	while (cursor.hasNext()) {
		System.out.println(cursor.next());
	} 
```

*输出*

```java
 { "name" : { "$regex" : "Mky.*-[1-3]" , "$options" : "i"}}

{ "_id" : { "$oid" : "515ad59e3004c89329c7b259"} , "number" : 1 , "name" : "mkyong-1"}
{ "_id" : { "$oid" : "515ad59e3004c89329c7b25a"} , "number" : 2 , "name" : "mkyong-2"}
{ "_id" : { "$oid" : "515ad59e3004c89329c7b25b"} , "number" : 3 , "name" : "mkyong-3"} 
```

**There are more…**
Read this [MongoDB operator documentation](http://web.archive.org/web/20190227120409/http://docs.mongodb.org/manual/reference/operators/) for complete set of query operators supported in MongoDB.

## 5.完整示例

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.List;

import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.DBObject;
import com.mongodb.Mongo;
import com.mongodb.MongoException;

/**
 * Java MongoDB : Query document
 * 
 * @author mkyong
 * 
 */
public class QueryApp {

	public static void insertDummyDocuments(DBCollection collection) {

		List<DBObject> list = new ArrayList<DBObject>();

		Calendar cal = Calendar.getInstance();

		for (int i = 1; i <= 5; i++) {

			BasicDBObject data = new BasicDBObject();
			data.append("number", i);
			data.append("name", "mkyong-" + i);
			// data.append("date", cal.getTime());

			// +1 day
			cal.add(Calendar.DATE, 1);

			list.add(data);

		}

		collection.insert(list);

	}

	public static void main(String[] args) {

	try {

	  Mongo mongo = new Mongo("localhost", 27017);
	  DB db = mongo.getDB("yourdb");

	  // get a single collection
	  DBCollection collection = db.getCollection("dummyColl");

	  insertDummyDocuments(collection);

	  System.out.println("1\. Find first matched document");
	  DBObject dbObject = collection.findOne();
	  System.out.println(dbObject);

	  System.out.println("\n1\. Find all matched documents");
	  DBCursor cursor = collection.find();
	  while (cursor.hasNext()) {
		System.out.println(cursor.next());
	  }

	  System.out.println("\n1\. Get 'name' field only");
	  BasicDBObject allQuery = new BasicDBObject();
	  BasicDBObject fields = new BasicDBObject();
	  fields.put("name", 1);

	  DBCursor cursor2 = collection.find(allQuery, fields);
	  while (cursor2.hasNext()) {
		System.out.println(cursor2.next());
	  }

	  System.out.println("\n2\. Find where number = 5");
	  BasicDBObject whereQuery = new BasicDBObject();
	  whereQuery.put("number", 5);
	  DBCursor cursor3 = collection.find(whereQuery);
	  while (cursor3.hasNext()) {
		System.out.println(cursor3.next());
	  }

	  System.out.println("\n2\. Find where number in 2,4 and 5");
	  BasicDBObject inQuery = new BasicDBObject();
	  List<Integer> list = new ArrayList<Integer>();
	  list.add(2);
	  list.add(4);
	  list.add(5);
	  inQuery.put("number", new BasicDBObject("$in", list));
	  DBCursor cursor4 = collection.find(inQuery);
	  while (cursor4.hasNext()) {
		System.out.println(cursor4.next());
	  }

	  System.out.println("\n2\. Find where 5 > number > 2");
	  BasicDBObject gtQuery = new BasicDBObject();
	  gtQuery.put("number", new BasicDBObject("$gt", 2).append("$lt", 5));
	  DBCursor cursor5 = collection.find(gtQuery);
	  while (cursor5.hasNext()) {
		System.out.println(cursor5.next());
	  }

	  System.out.println("\n2\. Find where number != 4");
	  BasicDBObject neQuery = new BasicDBObject();
	  neQuery.put("number", new BasicDBObject("$ne", 4));
	  DBCursor cursor6 = collection.find(neQuery);
	  while (cursor6.hasNext()) {
		System.out.println(cursor6.next());
	  }

	  System.out.println("\n3\. Find when number = 2 and name = 'mkyong-2' example");
	  BasicDBObject andQuery = new BasicDBObject();

	  List<BasicDBObject> obj = new ArrayList<BasicDBObject>();
	  obj.add(new BasicDBObject("number", 2));
	  obj.add(new BasicDBObject("name", "mkyong-2"));
	  andQuery.put("$and", obj);

	  System.out.println(andQuery.toString());

	  DBCursor cursor7 = collection.find(andQuery);
	  while (cursor7.hasNext()) {
		System.out.println(cursor7.next());
	  }

	  System.out.println("\n4\. Find where name = 'Mky.*-[1-3]', case sensitive example");
	  BasicDBObject regexQuery = new BasicDBObject();
	  regexQuery.put("name",
		new BasicDBObject("$regex", "Mky.*-[1-3]")
                    .append("$options", "i"));

	  System.out.println(regexQuery.toString());

	  DBCursor cursor8 = collection.find(regexQuery);
	  while (cursor8.hasNext()) {
		System.out.println(cursor8.next());
	  }

	  collection.drop();

	  System.out.println("Done");

	 } catch (UnknownHostException e) {
		e.printStackTrace();
	 } catch (MongoException e) {
		e.printStackTrace();
	 }

	}
} 
```

完成了。

## 参考

1.  [查询、更新和投影运算符快速参考](http://web.archive.org/web/20190227120409/http://docs.mongodb.org/manual/reference/operators/)

[mongodb](http://web.archive.org/web/20190227120409/http://www.mkyong.com/tag/mongodb/) [query](http://web.archive.org/web/20190227120409/http://www.mkyong.com/tag/query/)







