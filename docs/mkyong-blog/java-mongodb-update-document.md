# Java MongoDB:更新文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-mongodb-update-document/>

在本教程中，我们将向您展示如何使用 Java MongoDB API `collection.update()`来更新文档。

## 测试数据

假设插入了以下数据/文件。

```java
 {
	"hosting" : "hostA",
	"type" : "vps",
	"clients" : 1000
},
{
	"hosting" : "hostB",
	"type" : "dedicated server",
	"clients" : 100
},
{
	"hosting" : "hostC",
	"type" : "vps",
	"clients" : 900
} 
```

```java
 { "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "type" : "vps" , "clients" : 1000}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 100}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900} 
```

 ## 1.带有 set 的 DBCollection.update()

查找 hosting = 'hostB '的文档，并将其客户端值从 100 更新为 110。

```java
 BasicDBObject newDocument = new BasicDBObject();
	newDocument.put("clients", 110);

	BasicDBObject searchQuery = new BasicDBObject().append("hosting", "hostB");

	collection.update(searchQuery, newDocument); 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "type" : "vps" , "clients" : 1000}
{ "_id" : { "$oid" : "id"} , "clients" : 110}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900} 
```

**The document is replaced!?**
Wait, the entire “hostB” document is replaced with another new document, this is not what we want.

要仅更新特定值，使用`$set`更新修改器。

```java
 BasicDBObject newDocument = new BasicDBObject();
	newDocument.append("$set", new BasicDBObject().append("clients", 110));

	BasicDBObject searchQuery = new BasicDBObject().append("hosting", "hostB");

	collection.update(searchQuery, newDocument); 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "type" : "vps" , "clients" : 1000}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 110}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900} 
```

**Note**
The MongoDB team should create another new API named `DBCollection.replace()`, many beginners are trapped in this `DBCollection.update()` API and replace the entire document accidentally. Again, to update a particular value, use `$set`. ## 2.带有$inc .的 DBCollection.update()

这个例子显示了使用`$inc`修饰符来增加一个特定的值。查找 hosting = 'hostB '的文档，通过将值从 100 增加到 199，(100 + 99) = 199 来更新它的' clients '值。

```java
 BasicDBObject newDocument = 
		new BasicDBObject().append("$inc", 
		new BasicDBObject().append("total clients", 99));

	collection.update(new BasicDBObject().append("hosting", "hostB"), newDocument); 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "type" : "vps" , "clients" : 1000}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 199}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900} 
```

## 3.带有多个的 DBCollection.update()

这个例子展示了使用`multi`参数来更新一组匹配的文档。查找 type = 'vps '的文档，将所有匹配文档的' clients '值更新为 888。

```java
 BasicDBObject updateQuery = new BasicDBObject();
	updateQuery.append("$set", 
		new BasicDBObject().append("clients", "888"));

	BasicDBObject searchQuery = new BasicDBObject();
	searchQuery.append("type", "vps");

	collection.updateMulti(searchQuery, updateQuery);			

	//below statement set multi to true.
	//collection.update(searchQuery, updateQuery, false, true); 
```

*输出*

```java
 { "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "clients" : "888" , "type" : "vps"}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 100}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "clients" : "888" , "type" : "vps"} 
```

**Note**
If update without the `multi` set to true.

```java
 BasicDBObject updateQuery = new BasicDBObject();
	updateQuery.append("$set", 
		new BasicDBObject().append("clients", "888"));

	BasicDBObject searchQuery = new BasicDBObject();
	searchQuery.append("type", "vps");

	collection.update(searchQuery, updateQuery); 
```

您会注意到只有第一个匹配的文档被更新。

```java
 {"_id":{ "$oid" : "x"} , "hosting" : "hostA" , "clients" : "888" , "type" : "vps"}
{"_id":{ "$oid" : "x"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 100}
{"_id":{ "$oid" : "x"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900} 
```

要更新一组匹配的文档，需要将“`multi`”设置为 true。

## 4.完整示例

完整的例子结合上述代码片段。

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.Mongo;
import com.mongodb.MongoException;

/**
 * Java MongoDB update document
 * 
 * @author mkyong
 * 
 */

public class UpdateApp {

	public static void printAllDocuments(DBCollection collection) {
		DBCursor cursor = collection.find();
		while (cursor.hasNext()) {
			System.out.println(cursor.next());
		}
	}

	public static void removeAllDocuments(DBCollection collection) {
		collection.remove(new BasicDBObject());
	}

	public static void insertDummyDocuments(DBCollection collection) {
		BasicDBObject document = new BasicDBObject();
		document.put("hosting", "hostA");
		document.put("type", "vps");
		document.put("clients", 1000);

		BasicDBObject document2 = new BasicDBObject();
		document2.put("hosting", "hostB");
		document2.put("type", "dedicated server");
		document2.put("clients", 100);

		BasicDBObject document3 = new BasicDBObject();
		document3.put("hosting", "hostC");
		document3.put("type", "vps");
		document3.put("clients", 900);

		collection.insert(document);
		collection.insert(document2);
		collection.insert(document3);
	}

	public static void main(String[] args) {

	try {

	  Mongo mongo = new Mongo("localhost", 27017);
	  DB db = mongo.getDB("yourdb");

	  // get a single collection
	  DBCollection collection = db.getCollection("dummyColl");

	  System.out.println("Testing 1...no $set");

	  insertDummyDocuments(collection);

	  // find hosting = hostB, and update the clients to 110
	  BasicDBObject newDocument = new BasicDBObject();
	  newDocument.put("clients", 110);

	  BasicDBObject searchQuery = new BasicDBObject().append("hosting", "hostB");

	  collection.update(searchQuery, newDocument);

	  printAllDocuments(collection);
	  removeAllDocuments(collection);

	  System.out.println("\nTesting 1...with $set");

	  insertDummyDocuments(collection);

	  BasicDBObject updateDocument = new BasicDBObject();
	  updateDocument.append("$set", new BasicDBObject().append("clients", 110));

	  BasicDBObject searchQuery2 = new BasicDBObject().append("hosting", "hostB");

	  collection.update(searchQuery2, updateDocument);

	  printAllDocuments(collection);
	  removeAllDocuments(collection);

	  System.out.println("\nTesting 2... with $inc");
	  insertDummyDocuments(collection);
	  // find hosting = hostB and increase it's "clients" value by 99
	  BasicDBObject newDocument2 = new BasicDBObject().append("$inc",
		new BasicDBObject().append("clients", 99));

	  collection.update(new BasicDBObject().append("hosting", "hostB"), newDocument2);

	  printAllDocuments(collection);
	  removeAllDocuments(collection);

	  System.out.println("\nTesting 3... with $multi");

	  insertDummyDocuments(collection);
	  // find type = vps , update all matched documents , clients value to 888
	  BasicDBObject updateQuery = new BasicDBObject();
	  updateQuery.append("$set", new BasicDBObject().append("clients", "888"));

	  BasicDBObject searchQuery3 = new BasicDBObject();
	  searchQuery3.append("type", "vps");

	  collection.updateMulti(searchQuery3, updateQuery);
	  // collection.update(searchQuery3, updateQuery, false, true);

	  printAllDocuments(collection);
	  removeAllDocuments(collection);

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
 Testing 1...no $set
{ "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "type" : "vps" , "clients" : 1000}
{ "_id" : { "$oid" : "id"} , "clients" : 110}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900}

Testing 1...with $set
{ "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "type" : "vps" , "clients" : 1000}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 110}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900}

Testing 2... with $inc
{ "_id" : { "$oid" : "id"} , "hosting" : "hostA" , "type" : "vps" , "clients" : 1000}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 199}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900}

Testing 3... with $multi
{ "_id" : { "$oid" : "id"} , "hosting" : "hostB" , "type" : "dedicated server" , "clients" : 100}
{ "_id" : { "$oid" : "id"} , "hosting" : "hostC" , "type" : "vps" , "clients" : 900}
{ "_id" : { "$oid" : "id"} , "clients" : "888" , "hosting" : "hostA" , "type" : "vps"}
Done 
```

## 参考

1.  [如何在 MongoDB 中进行更新](http://web.archive.org/web/20190219073346/http://docs.mongodb.org/manual/applications/update/)
2.  [$设置更新修改量](http://web.archive.org/web/20190219073346/http://docs.mongodb.org/manual/reference/operator/set/#op._S_set)
3.  [$inc 更新修改量](http://web.archive.org/web/20190219073346/http://docs.mongodb.org/manual/reference/operator/inc/#op._S_inc)
4.  [Java MongoDB API，DBCollection JavaDoc](http://web.archive.org/web/20190219073346/http://api.mongodb.org/java/current/com/mongodb/DBCollection.html)

[mongodb](http://web.archive.org/web/20190219073346/http://www.mkyong.com/tag/mongodb/) [update](http://web.archive.org/web/20190219073346/http://www.mkyong.com/tag/update/)







