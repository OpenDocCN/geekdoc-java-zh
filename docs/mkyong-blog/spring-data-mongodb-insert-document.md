# Spring Data MongoDB:插入文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/spring-data-mongodb-insert-document/>

在 Spring data MongoDB 中，可以使用`save()`、`insert()`将一个或一列对象保存到 MongoDB 数据库中。

```java
 User user = new User("...");

	//save user object into "user" collection / table
	//class name will be used as collection name
	mongoOperation.save(user);

	//save user object into "tableA" collection
	mongoOperation.save(user,"tableA");

	//insert user object into "user" collection
	//class name will be used as collection name
	mongoOperation.insert(user);

	//insert user object into "tableA" collection
	mongoOperation.insert(user, "tableA");

	//insert a list of user objects
	mongoOperation.insert(listofUser); 
```

默认情况下，如果你保存了一个对象，并且没有指定任何“集合名”，类名将被用作集合名。

## 1.保存并插入

你应该使用保存还是插入？

1.  保存–应该重命名为`saveOrUpdate()`，如果“_id”不存在，则执行`insert()`，如果“_id”存在，则执行`update()`。
2.  insert–仅插入，如果存在“_id ”,则产生错误。

参见下面的例子

```java
 //get an existed data, and update it
	User userD1 = mongoOperation.findOne(
		new Query(Criteria.where("age").is(64)), User.class);
	userD1.setName("new name");
	userD1.setAge(100);

	//if you insert it, 'E11000 duplicate key error index' error is generated.
	//mongoOperation.insert(userD1); 

	//instead you should use save.
	mongoOperation.save(userD1); 
```

## 2.插入文档示例

查看一个完整的示例，向您展示如何将一个或一个“用户”对象列表保存到 MongoDB 中。

SpringMongoConfig.java – Create a mongoTemplate bean in Spring container.

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.mongodb.core.MongoTemplate;
import com.mongodb.MongoClient;

/**
 * Spring MongoDB configuration file
 * 
 */
@Configuration
public class SpringMongoConfig{

	public @Bean
	MongoTemplate mongoTemplate() throws Exception {

		MongoTemplate mongoTemplate = 
			new MongoTemplate(new MongoClient("127.0.0.1"),"yourdb");
		return mongoTemplate;

	}

} 
```

当您保存该对象时，使用@Document 定义一个“集合名称”。在这种情况下，当“用户”对象保存时，它将保存到“用户”集合中。

User.java

```java
 package com.mkyong.user;

import java.util.Date;
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.index.Indexed;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.format.annotation.DateTimeFormat.ISO;

@Document(collection = "users")
public class User {

	@Id
	private String id;

	@Indexed
	private String ic;

	private String name;

	private int age;

	@DateTimeFormat(iso = ISO.DATE_TIME)
	private Date createdDate;

	//getter and setter methods
} 
```

完整的例子，向您展示不同的方式插入数据，阅读代码和注释，不言自明。

App.java

```java
 package com.mkyong.core;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.data.mongodb.core.MongoOperations;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;

import com.mkyong.config.SpringMongoConfig;
import com.mkyong.user.User;

public class App {

	public static void main(String[] args) {
		// For Annotation
		ApplicationContext ctx = 
                     new AnnotationConfigApplicationContext(SpringMongoConfig.class);
		MongoOperations mongoOperation = 
                     (MongoOperations) ctx.getBean("mongoTemplate");

		// Case1 - insert a user, put "tableA" as collection name
		System.out.println("Case 1...");
		User userA = new User("1000", "apple", 54, new Date());
		mongoOperation.save(userA, "tableA");

		// find
		Query findUserQuery = new Query();
		findUserQuery.addCriteria(Criteria.where("ic").is("1000"));
		User userA1 = mongoOperation.findOne(findUserQuery, User.class, "tableA");
		System.out.println(userA1);

		// Case2 - insert a user, put entity as collection name
		System.out.println("Case 2...");
		User userB = new User("2000", "orange", 64, new Date());
		mongoOperation.save(userB);

		// find
		User userB1 = mongoOperation.findOne(
                     new Query(Criteria.where("age").is(64)), User.class);
		System.out.println(userB1);

		// Case3 - insert a list of users
		System.out.println("Case 3...");
		User userC = new User("3000", "metallica", 34, new Date());
		User userD = new User("4000", "metallica", 34, new Date());
		User userE = new User("5000", "metallica", 34, new Date());
		List<User> userList = new ArrayList<User>();
		userList.add(userC);
		userList.add(userD);
		userList.add(userE);
		mongoOperation.insert(userList, User.class);

		// find
		List<User> users = mongoOperation.find(
                           new Query(Criteria.where("name").is("metallica")),
			   User.class);

		for (User temp : users) {
			System.out.println(temp);
		}

		//save vs insert
		System.out.println("Case 4...");
		User userD1 = mongoOperation.findOne(
                          new Query(Criteria.where("age").is(64)), User.class);
		userD1.setName("new name");
		userD1.setAge(100);

		//E11000 duplicate key error index, _id existed
		//mongoOperation.insert(userD1); 
		mongoOperation.save(userD1);
		User userE1 = mongoOperation.findOne(
                         new Query(Criteria.where("age").is(100)), User.class);
		System.out.println(userE1);
	}

} 
```

*输出*

```java
 Case 1...
User [id=id, ic=1000, name=apple, age=54, createdDate=Sat Apr 06 12:35:15 MYT 2013]
Case 2...
User [id=id, ic=2000, name=orange, age=64, createdDate=Sat Apr 06 12:59:19 MYT 2013]
Case 3...
User [id=id, ic=3000, name=metallica, age=34, createdDate=Sat Apr 06 12:59:19 MYT 2013]
User [id=id, ic=4000, name=metallica, age=34, createdDate=Sat Apr 06 12:59:19 MYT 2013]
User [id=id, ic=5000, name=metallica, age=34, createdDate=Sat Apr 06 12:59:19 MYT 2013]
Case 4...
User [id=id, ic=2000, name=new name, age=100, createdDate=Sat Apr 06 12:59:19 MYT 2013] 
```

## 3.Mongo 控制台

查看 Mongo 控制台，看看插入和创建了什么。

```java
 > mongo
MongoDB shell version: 2.2.3
connecting to: test

> show dbs
admin	0.203125GB
yourdb	0.203125GB

> use yourdb
switched to db yourdb
> show collections
system.indexes
tableA
users

> db.tableA.find()
{ "_id" : ObjectId("id"), "_class" : "com.mkyong.user.User", 
"ic" : "1000", "name" : "apple", "age" : 54, "createdDate" : ISODate("2013-04-06T05:04:06.384Z") }

> db.users.find()
{ "_id" : ObjectId("id"), "_class" : "com.mkyong.user.User", 
"ic" : "3000", "name" : "metallica", "age" : 34, "createdDate" : ISODate("2013-04-06T05:04:06.735Z") }
{ "_id" : ObjectId("id"), "_class" : "com.mkyong.user.User", 
"ic" : "4000", "name" : "metallica", "age" : 34, "createdDate" : ISODate("2013-04-06T05:04:06.735Z") }
{ "_id" : ObjectId("id"), "_class" : "com.mkyong.user.User", 
"ic" : "5000", "name" : "metallica", "age" : 34, "createdDate" : ISODate("2013-04-06T05:04:06.735Z") }
{ "_id" : ObjectId("id"), "_class" : "com.mkyong.user.User", 
"ic" : "2000", "name" : "new name", "age" : 100, "createdDate" : ISODate("2013-04-06T05:04:06.731Z") } 
```

页（page 的缩写）s 要移除 extra _class 列，请阅读本文—[Spring Data MongoDB Remove _ class 列](http://web.archive.org/web/20221225035509/http://www.mkyong.com/mongodb/spring-data-mongodb-remove-_class-column/)。

## 下载源代码

Download it – [SpringData-MongoDB-Insert-Example.zip](http://web.archive.org/web/20221225035509/http://www.mkyong.com/wp-content/uploads/2011/05/SpringMongoDB-Insert-Example.zip) (24KB)

## 参考

1.  [Spring Data MongoDB–保存、更新和删除文档](http://web.archive.org/web/20221225035509/http://static.springsource.org/spring-data/mongodb/docs/current/reference/html/mongo.core.html#mongo-template.save-update-remove)
2.  [春季数据 MongoDB Hello World 示例](http://web.archive.org/web/20221225035509/http://www.mkyong.com/mongodb/spring-data-mongodb-hello-world-example/)

<input type="hidden" id="mkyong-current-postId" value="8878">