# Java MongoDB:删除文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-mongodb-delete-document/>

在本教程中，我们将向您展示如何使用`collection.remove()`从集合中删除文档。

## 1.测试数据

从 1 号到 10 号插入 10 份文件进行测试。

```java
 for (int i=1; i <= 10; i++) {
    collection.insert(new BasicDBObject().append("number", i));
} 
```

 ## 2.DBCollection.remove()

参见下面删除文档的代码片段。

##### 示例 1

获取第一个文档并删除它。在这种情况下，number = 1 被删除。

```java
 DBObject doc = collection.findOne(); //get first document
	collection.remove(doc); 
```

##### 示例 2

将查询放在一个`BasicDBObject`中。在这种情况下，number = 2 被删除。

```java
 BasicDBObject document = new BasicDBObject();
	document.put("number", 2);
	collection.remove(document); 
```

**And Operator?**

两个常见错误:

1.像这样的查询只删除数字= 3。

```java
 BasicDBObject document = new BasicDBObject();
	document.put("number", 2);
        document.put("number", 3); //override above value 2
	collection.remove(document); 
```

2.下面的尝试很好，但是像这样的查询不起作用，它不会删除任何东西。

```java
 BasicDBObject document = new BasicDBObject();
	List<Integer> list = new ArrayList<Integer>();
	list.add(7);
	list.add(8);
	document.put("number", list);
	collection.remove(document); 
```

对于“与”查询，需要使用“$in”或“$and”运算符，参见例 5。

##### 示例 3

直接用`BasicDBObject`。在这种情况下，number = 3 被删除。

```java
 collection.remove(new BasicDBObject().append("number", 3)); 
```

##### 实例 4

将一个`$gt`操作符放在一个`BasicDBObject`对象中。在这种情况下，number = 10 被删除。

```java
 BasicDBObject query = new BasicDBObject();
	query.put("number", new BasicDBObject("$gt", 9));
	collection.remove(query); 
```

##### 实例 5

将一个`$in`操作符放在一个`BasicDBObject`对象中，在 ArrayList 中构造查询。在这种情况下，number = 4 和 number = 5 被删除。

```java
 BasicDBObject query2 = new BasicDBObject();
	List<Integer> list = new ArrayList<Integer>();
	list.add(4);
	list.add(5);
	query2.put("number", new BasicDBObject("$in", list));
	collection.remove(query2); 
```

**More MongoDB Operators**
For more operators, read this [MongoDB operators quick reference](http://web.archive.org/web/20190220141530/http://docs.mongodb.org/manual/reference/operators/#comparison).

##### 实例 6

使用光标删除所有可用的文档。(不推荐，首选示例 7)

```java
 DBCursor cursor = collection.find();
	while (cursor.hasNext()) {
		collection.remove(cursor.next());
	} 
```

##### 例 7

传递一个空的 BasicDBObject，整个文档将被删除。

```java
 collection.remove(new BasicDBObject()); 
```

##### 实施例 8

它删除整个文档并丢弃集合。

```java
 collection.drop(); 
```

##### 示例 9

`remove()`将返回一个`WrireResult`对象，它包含关于移除操作的有用信息。您可以使用`getN()`来获取受影响的文档数量。

```java
 WriteResult result = collection.remove(query2);
       System.out.println("Number of documents are deleted : " + result.getN()); 
```

 ## 3.完整示例

完整的例子显示了不同的方式来删除文件。

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import java.util.ArrayList;
import java.util.List;

import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.DBObject;
import com.mongodb.Mongo;
import com.mongodb.MongoException;

/**
 * Java MongoDB : Delete document
 * @author mkyong
 */

public class App {
    public static void main(String[] args) {

      try {

	Mongo mongo = new Mongo("localhost", 27017);
	DB db = mongo.getDB("yourdb");

	// get a single collection
	DBCollection collection = db.getCollection("dummyColl");

	//insert number 1 to 10 for testing
	for (int i=1; i <= 10; i++) {
		collection.insert(new BasicDBObject().append("number", i));
	}

	//remove number = 1
	DBObject doc = collection.findOne(); //get first document
	collection.remove(doc);

	//remove number = 2
	BasicDBObject document = new BasicDBObject();
	document.put("number", 2);
	collection.remove(document);

	//remove number = 3
	collection.remove(new BasicDBObject().append("number", 3));

	//remove number > 9 , means delete number = 10
	BasicDBObject query = new BasicDBObject();
	query.put("number", new BasicDBObject("$gt", 9));
	collection.remove(query);

	//remove number = 4 and 5
	BasicDBObject query2 = new BasicDBObject();
	List<Integer> list = new ArrayList<Integer>();
	list.add(4);
	list.add(5);
	query2.put("number", new BasicDBObject("$in", list));
	collection.remove(query2);

	//print out the document
	DBCursor cursor = collection.find();
        while(cursor.hasNext()) {
             System.out.println(cursor.next());
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

输出...

```java
 { "_id" : { "$oid" : "4dc7a6989e3a66c5faeee757"} , "number" : 6}
{ "_id" : { "$oid" : "4dc7a6989e3a66c5faeee758"} , "number" : 7}
{ "_id" : { "$oid" : "4dc7a6989e3a66c5faeee759"} , "number" : 8}
{ "_id" : { "$oid" : "4dc7a6989e3a66c5faeee75a"} , "number" : 9}
Done 
```

## 参考

1.  [MongoDB 操作员快速参考](http://web.archive.org/web/20190220141530/http://docs.mongodb.org/manual/reference/operators/)
2.  [MongoDB write result JavaDoc](http://web.archive.org/web/20190220141530/http://api.mongodb.org/java/2.6.5/com/mongodb/WriteResult.html)

[delete](http://web.archive.org/web/20190220141530/http://www.mkyong.com/tag/delete/) [mongodb](http://web.archive.org/web/20190220141530/http://www.mkyong.com/tag/mongodb/)







