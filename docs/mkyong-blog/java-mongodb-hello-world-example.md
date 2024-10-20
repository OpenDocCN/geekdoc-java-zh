# Java + MongoDB hello world 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-mongodb-hello-world-example/>

![Java mongodb hello world](img/68fdec967ae7f4780c964ca910875ecc.png)

一个简单的 Java + MongoDB hello world 例子——如何连接、创建数据库、集合和文档、保存、更新、删除、获取和显示文档(数据)。

使用的工具和技术:

1.  MongoDB 2.2.3
2.  MongoDB-Java-驱动程序 2.10.1
3.  JDK 1.6
4.  Maven 3.0.3
5.  Eclipse 4.2

P.S Maven 和 Eclipse 都是可选的，只是我个人最喜欢的开发工具。

## 1.创建一个 Java 项目

用 Maven 创建一个[简单的 Java 项目](http://web.archive.org/web/20190220123320/http://www.mkyong.com/maven/how-to-create-a-java-project-with-maven/)。

```java
 mvn archetype:generate -DgroupId=com.mkyong.core -DartifactId=mongodb 
  -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
```

 ## 2.获取 Mongo Java 驱动程序

从 [github](http://web.archive.org/web/20190220123320/https://github.com/mongodb/mongo-java-driver/downloads) 下载 mongo-java 驱动。对于 Maven 用户，mongo-java 驱动程序在`pom.xml`中声明。

pom.xml

```java
 <project ...>
  <dependencies>

	<dependency>
		<groupId>org.mongodb</groupId>
		<artifactId>mongo-java-driver</artifactId>
		<version>2.10.1</version>
	</dependency>

  </dependencies>

  <build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>2.3.1</version>
			<configuration>
				<source>1.6</source>
				<target>1.6</target>
			</configuration>
		</plugin>
	        <plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-eclipse-plugin</artifactId>
			<configuration>
				<downloadSources>true</downloadSources>
				<downloadJavadocs>true</downloadJavadocs>
			</configuration>
		</plugin>

	</plugins>
  </build>

</project> 
```

 ## 3.Mongo 连接

连接到 MongoDB 服务器。对于 MongoDB 版本> = 2.10.0，使用`MongoClient`。

```java
 // Old version, uses Mongo
	Mongo mongo = new Mongo("localhost", 27017);

	// Since 2.10.0, uses MongoClient
	MongoClient mongo = new MongoClient( "localhost" , 27017 ); 
```

如果 MongoDB 处于安全模式，则需要身份验证。

```java
 MongoClient mongoClient = new MongoClient();
	DB db = mongoClient.getDB("database name");
	boolean auth = db.authenticate("username", "password".toCharArray()); 
```

## 4.Mongo 数据库

获取数据库。如果数据库不存在，MongoDB 会为您创建一个。

```java
 DB db = mongo.getDB("database name"); 
```

显示所有数据库。

```java
 List<String> dbs = mongo.getDatabaseNames();
	for(String db : dbs){
		System.out.println(db);
	} 
```

## 5.Mongo 系列

获取集合/表格。

```java
 DB db = mongo.getDB("testdb");
	DBCollection table = db.getCollection("user"); 
```

显示所选数据库中的所有集合。

```java
 DB db = mongo.getDB("testdb");
	Set<String> tables = db.getCollectionNames();

	for(String coll : tables){
		System.out.println(coll);
	} 
```

**Note**
In RDBMS, collection is equal to table.

## 6.保存示例

将文档(数据)保存到名为“user”的集合(表)中。

```java
 DBCollection table = db.getCollection("user");
	BasicDBObject document = new BasicDBObject();
	document.put("name", "mkyong");
	document.put("age", 30);
	document.put("createdDate", new Date());
	table.insert(document); 
```

参考这个 [Java MongoDB 插入示例](http://web.archive.org/web/20190220123320/http://www.mkyong.com/mongodb/java-mongodb-insert-a-document/)。

## 7.更新示例

更新一个文档，其中“name=mkyong”。

```java
 DBCollection table = db.getCollection("user");

	BasicDBObject query = new BasicDBObject();
	query.put("name", "mkyong");

	BasicDBObject newDocument = new BasicDBObject();
	newDocument.put("name", "mkyong-updated");

	BasicDBObject updateObj = new BasicDBObject();
	updateObj.put("$set", newDocument);

	table.update(query, updateObj); 
```

参考这个 [Java MongoDB 更新实例](http://web.archive.org/web/20190220123320/http://www.mkyong.com/mongodb/java-mongodb-update-document/)。

## 8.查找示例

找到“name=mkyong”处的文档，并用 DBCursor 显示它

```java
 DBCollection table = db.getCollection("user");

	BasicDBObject searchQuery = new BasicDBObject();
	searchQuery.put("name", "mkyong");

	DBCursor cursor = table.find(searchQuery);

	while (cursor.hasNext()) {
		System.out.println(cursor.next());
	} 
```

参考这个 [Java MongoDB 搜索查询例子](http://web.archive.org/web/20190220123320/http://www.mkyong.com/mongodb/java-mongodb-query-document/)。

## 9.删除示例

找到“name=mkyong”处的文档，并删除它。

```java
 DBCollection table = db.getCollection("user");

	BasicDBObject searchQuery = new BasicDBObject();
	searchQuery.put("name", "mkyong");

	table.remove(searchQuery); 
```

参考这个 [Java MongoDB 删除例子](http://web.archive.org/web/20190220123320/http://www.mkyong.com/mongodb/java-mongodb-delete-document/)。

## 10.你好世界

让我们回顾一个完整的 Java + MongoDB 示例，不言自明的请参见注释。

App.java

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import java.util.Date;
import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.MongoClient;
import com.mongodb.MongoException;

/**
 * Java + MongoDB Hello world Example
 * 
 */
public class App {
  public static void main(String[] args) {

    try {

	/**** Connect to MongoDB ****/
	// Since 2.10.0, uses MongoClient
	MongoClient mongo = new MongoClient("localhost", 27017);

	/**** Get database ****/
	// if database doesn't exists, MongoDB will create it for you
	DB db = mongo.getDB("testdb");

	/**** Get collection / table from 'testdb' ****/
	// if collection doesn't exists, MongoDB will create it for you
	DBCollection table = db.getCollection("user");

	/**** Insert ****/
	// create a document to store key and value
	BasicDBObject document = new BasicDBObject();
	document.put("name", "mkyong");
	document.put("age", 30);
	document.put("createdDate", new Date());
	table.insert(document);

	/**** Find and display ****/
	BasicDBObject searchQuery = new BasicDBObject();
	searchQuery.put("name", "mkyong");

	DBCursor cursor = table.find(searchQuery);

	while (cursor.hasNext()) {
		System.out.println(cursor.next());
	}

	/**** Update ****/
	// search document where name="mkyong" and update it with new values
	BasicDBObject query = new BasicDBObject();
	query.put("name", "mkyong");

	BasicDBObject newDocument = new BasicDBObject();
	newDocument.put("name", "mkyong-updated");

	BasicDBObject updateObj = new BasicDBObject();
	updateObj.put("$set", newDocument);

	table.update(query, updateObj);

	/**** Find and display ****/
	BasicDBObject searchQuery2 
	    = new BasicDBObject().append("name", "mkyong-updated");

	DBCursor cursor2 = table.find(searchQuery2);

	while (cursor2.hasNext()) {
		System.out.println(cursor2.next());
	}

	/**** Done ****/
	System.out.println("Done");

    } catch (UnknownHostException e) {
	e.printStackTrace();
    } catch (MongoException e) {
	e.printStackTrace();
    }

  }
} 
```

输出…

```java
 { "_id" : { "$oid" : "51398e6e30044a944cc23e2e"} , "name" : "mkyong" , "age" : 30 , "createdDate" : { "$date" : "2013-03-08T07:08:30.168Z"}}
{ "_id" : { "$oid" : "51398e6e30044a944cc23e2e"} , "age" : 30 , "createdDate" : { "$date" : "2013-03-08T07:08:30.168Z"} , "name" : "mkyong-updated"}
Done 
```

让我们使用`mongo`控制台来检查创建的数据库“testdb”、集合“user”和文档。

```java
 $ mongo
MongoDB shell version: 2.2.3
connecting to: test

> show dbs
testdb	0.203125GB

> use testdb
switched to db testdb

> show collections
system.indexes
user
> db.user.find()
{ "_id" : ObjectId("51398e6e30044a944cc23e2e"), "age" : 30, "createdDate" : ISODate("2013-03-08T07:08:30.168Z"), "name" : "mkyong-updated" } 
```

## 下载源代码

Download it – [Java-mongodb-hello-world-example.zip](http://web.archive.org/web/20190220123320/http://www.mkyong.com/wp-content/uploads/2011/05/Java-mongodb-hello-world-example.zip) (13KB)

## 参考

1.  【Java 驱动程序入门
2.  [Java-MongoDB 驱动程序](http://web.archive.org/web/20190220123320/https://github.com/mongodb/mongo-java-driver/downloads)

[java](http://web.archive.org/web/20190220123320/http://www.mkyong.com/tag/java/) [mongodb](http://web.archive.org/web/20190220123320/http://www.mkyong.com/tag/mongodb/)







