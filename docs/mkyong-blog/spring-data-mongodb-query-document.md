# Spring Data MongoDB:查询文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/spring-data-mongodb-query-document/>

在这里，我们向您展示了一些从 MongoDB 查询文档的例子，使用了 query、Criteria 和一些常见的操作符。

## 测试数据

```java
 > db.users.find()
{ "_id" : ObjectId("id"), "ic" : "1001", "name" : "ant", "age" : 10 }
{ "_id" : ObjectId("id"), "ic" : "1002", "name" : "bird", "age" : 20 }
{ "_id" : ObjectId("id"), "ic" : "1003", "name" : "cat", "age" : 30 }
{ "_id" : ObjectId("id"), "ic" : "1004", "name" : "dog", "age" : 40 }
{ "_id" : ObjectId("id"), "ic" : "1005", "name" : "elephant", "age" : 50 }
{ "_id" : ObjectId("id"), "ic" : "1006", "name" : "frog", "age" : 60 } 
```

*注:本例在`mongo-java-driver-2.11.0.jar`和`spring-data-mongodb-1.2.0.RELEASE.jar`和*下测试

 ## 1.基本查询示例

如果您熟悉核心 MongoDB 控制台 find()命令，只需将“原始”查询放在`BasicQuery`中。

```java
 BasicQuery query1 = new BasicQuery("{ age : { $lt : 40 }, name : 'cat' }");
User userTest1 = mongoOperation.findOne(query1, User.class);

System.out.println("query1 - " + query1.toString());
System.out.println("userTest1 - " + userTest1); 
```

输出

```java
 query1 - Query: { "age" : { "$lt" : 40} , "name" : "cat"}, Fields: null, Sort: { }
userTest1 - User [id=id, ic=1003, name=cat, age=30] 
```

 ## 2.找一个例子

findOne 将返回与查询匹配的单个文档，您可以用`Criteria.and()`方法组合几个标准。更多细节见例 4。

```java
 Query query2 = new Query();
query2.addCriteria(Criteria.where("name").is("dog").and("age").is(40));

User userTest2 = mongoOperation.findOne(query2, User.class);
System.out.println("query2 - " + query2.toString());
System.out.println("userTest2 - " + userTest2); 
```

输出

```java
 query2 - Query: { "name" : "dog" , "age" : 40}, Fields: null, Sort: null
userTest2 - User [id=id, ic=1004, name=dog, age=40] 
```

## 3.查找和$inc 示例

查找并返回与查询匹配的文档列表。这个例子也展示了`$inc`操作符的使用。

```java
 List<Integer> listOfAge = new ArrayList<Integer>();
listOfAge.add(10);
listOfAge.add(30);
listOfAge.add(40);

Query query3 = new Query();
query3.addCriteria(Criteria.where("age").in(listOfAge));

List<User> userTest3 = mongoOperation.find(query3, User.class);
System.out.println("query3 - " + query3.toString());

for (User user : userTest3) {
	System.out.println("userTest3 - " + user);
} 
```

输出

```java
 query3 - Query: { "age" : { "$in" : [ 10 , 30 , 40]}}, Fields: null, Sort: null
userTest3 - User [id=id, ic=1001, name=ant, age=10]
userTest3 - User [id=id, ic=1003, name=cat, age=30]
userTest3 - User [id=id, ic=1004, name=dog, age=40] 
```

## 4.查找和$gt，$lt，$和示例

查找并返回与查询匹配的文档列表。这个例子还展示了`$gt`、`$lt`和`$and`操作符的使用。

```java
 Query query4 = new Query();
query4.addCriteria(Criteria.where("age").lt(40).and("age").gt(10));

List<User> userTest4 = mongoOperation.find(query4, User.class);
System.out.println("query4 - " + query4.toString());

for (User user : userTest4) {
	System.out.println("userTest4 - " + user);
} 
```

Oppss，生成一个错误消息，API 不这样工作:)

```java
 Due to limitations of the com.mongodb.BasicDBObject, you can't add a second 'age' expression 
specified as 'age : { "$gt" : 10}'. Criteria already contains 'age : { "$lt" : 40}'. 
```

您不能使用`Criteria.and()`将多个标准添加到同一个字段中，要解决这个问题，请使用`Criteria.andOperator()`，参见更新的示例:

```java
 Query query4 = new Query();
query4.addCriteria(
	Criteria.where("age").exists(true)
	.andOperator(
		Criteria.where("age").gt(10),
                Criteria.where("age").lt(40)
	)
);

List<User> userTest4 = mongoOperation.find(query4, User.class);
System.out.println("query4 - " + query4.toString());

for (User user : userTest4) {
	System.out.println("userTest4 - " + user);
} 
```

输出

```java
 query4 - Query: { "age" : { "$lt" : 40} , "$and" : [ { "age" : { "$gt" : 10}}]}, Fields: null, Sort: null
userTest4 - User [id=51627a0a3004cc5c0af72964, ic=1002, name=bird, age=20]
userTest4 - User [id=51627a0a3004cc5c0af72965, ic=1003, name=cat, age=30] 
```

## 5.查找和排序示例

查找并排序结果。

```java
 Query query5 = new Query();
query5.addCriteria(Criteria.where("age").gte(30));
query5.with(new Sort(Sort.Direction.DESC, "age"));

List<User> userTest5 = mongoOperation.find(query5, User.class);
System.out.println("query5 - " + query5.toString());

for (User user : userTest5) {
	System.out.println("userTest5 - " + user);
} 
```

输出

```java
 query5 - Query: { "age" : { "$gte" : 30}}, Fields: null, Sort: { "age" : -1}
userTest5 - User [id=id, ic=1006, name=frog, age=60]
userTest5 - User [id=id, ic=1005, name=elephant, age=50]
userTest5 - User [id=id, ic=1004, name=dog, age=40]
userTest5 - User [id=id, ic=1003, name=cat, age=30] 
```

## 6.find 和$regex 示例

通过正则表达式模式查找。

```java
 Query query6 = new Query();
query6.addCriteria(Criteria.where("name").regex("D.*G", "i"));

List<User> userTest6 = mongoOperation.find(query6, User.class);
System.out.println("query6 - " + query6.toString());

for (User user : userTest6) {
	System.out.println("userTest6 - " + user);
} 
```

输出

```java
 query6 - Query: { "name" : { "$regex" : "D.*G" , "$options" : "i"}}, Fields: null, Sort: null
userTest6 - User [id=id, ic=1004, name=dog, age=40] 
```

## 7.完整示例

一个完整的例子，结合了例 1 到例 6 中的所有内容。

SpringMongoConfig.java

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

User.java

```java
 package com.mkyong.model;

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

	//getter, setter and constructor methods

} 
```

QueryApp.java

```java
 package com.mkyong.core;

import java.util.ArrayList;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.data.domain.Sort;
import org.springframework.data.mongodb.core.MongoOperations;
import org.springframework.data.mongodb.core.query.BasicQuery;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;

import com.mkyong.config.SpringMongoConfig;
import com.mkyong.model.User;

/**
 * Query example
 * 
 * @author mkyong
 * 
 */
public class QueryApp {

	public static void main(String[] args) {

		ApplicationContext ctx = 
                         new AnnotationConfigApplicationContext(SpringMongoConfig.class);
		MongoOperations mongoOperation = 
                         (MongoOperations) ctx.getBean("mongoTemplate");

		// insert 6 users for testing
		List<User> users = new ArrayList<User>();

		User user1 = new User("1001", "ant", 10);
		User user2 = new User("1002", "bird", 20);
		User user3 = new User("1003", "cat", 30);
		User user4 = new User("1004", "dog", 40);
		User user5 = new User("1005", "elephant",50);
		User user6 = new User("1006", "frog", 60);
		users.add(user1);
		users.add(user2);
		users.add(user3);
		users.add(user4);
		users.add(user5);
		users.add(user6);
		mongoOperation.insert(users, User.class);

		System.out.println("Case 1 - find with BasicQuery example");

		BasicQuery query1 = new BasicQuery("{ age : { $lt : 40 }, name : 'cat' }");
		User userTest1 = mongoOperation.findOne(query1, User.class);

		System.out.println("query1 - " + query1.toString());
		System.out.println("userTest1 - " + userTest1);

		System.out.println("\nCase 2 - find example");

		Query query2 = new Query();
		query2.addCriteria(Criteria.where("name").is("dog").and("age").is(40));

		User userTest2 = mongoOperation.findOne(query2, User.class);
		System.out.println("query2 - " + query2.toString());
		System.out.println("userTest2 - " + userTest2);

		System.out.println("\nCase 3 - find list $inc example");

		List<Integer> listOfAge = new ArrayList<Integer>();
		listOfAge.add(10);
		listOfAge.add(30);
		listOfAge.add(40);

		Query query3 = new Query();
		query3.addCriteria(Criteria.where("age").in(listOfAge));

		List<User> userTest3 = mongoOperation.find(query3, User.class);
		System.out.println("query3 - " + query3.toString());

		for (User user : userTest3) {
			System.out.println("userTest3 - " + user);
		}

		System.out.println("\nCase 4 - find list $and $lt, $gt example");

		Query query4 = new Query();

		// it hits error
		// query4.addCriteria(Criteria.where("age").lt(40).and("age").gt(10));

		query4.addCriteria(
                   Criteria.where("age").exists(true).andOperator(
		         Criteria.where("age").gt(10),
                         Criteria.where("age").lt(40)
	            )
                );

		List<User> userTest4 = mongoOperation.find(query4, User.class);
		System.out.println("query4 - " + query4.toString());

		for (User user : userTest4) {
			System.out.println("userTest4 - " + user);
		}

		System.out.println("\nCase 5 - find list and sorting example");
		Query query5 = new Query();
		query5.addCriteria(Criteria.where("age").gte(30));
		query5.with(new Sort(Sort.Direction.DESC, "age"));

		List<User> userTest5 = mongoOperation.find(query5, User.class);
		System.out.println("query5 - " + query5.toString());

		for (User user : userTest5) {
			System.out.println("userTest5 - " + user);
		}

		System.out.println("\nCase 6 - find by regex example");
		Query query6 = new Query();
		query6.addCriteria(Criteria.where("name").regex("D.*G", "i"));

		List<User> userTest6 = mongoOperation.find(query6, User.class);
		System.out.println("query6 - " + query6.toString());

		for (User user : userTest6) {
			System.out.println("userTest6 - " + user);
		}

		mongoOperation.dropCollection(User.class);

	}

} 
```

输出

```java
 Case 1 - find with BasicQuery example
query1 - Query: { "age" : { "$lt" : 40} , "name" : "cat"}, Fields: null, Sort: { }
userTest1 - User [id=id, ic=1003, name=cat, age=30]

Case 2 - find example
query2 - Query: { "name" : "dog" , "age" : 40}, Fields: null, Sort: null
userTest2 - User [id=id, ic=1004, name=dog, age=40]

Case 3 - find list $inc example
query3 - Query: { "age" : { "$in" : [ 10 , 30 , 40]}}, Fields: null, Sort: null
userTest3 - User [id=id, ic=1001, name=ant, age=10]
userTest3 - User [id=id, ic=1003, name=cat, age=30]
userTest3 - User [id=id, ic=1004, name=dog, age=40]

Case 4 - find list $and $lt, $gt example
query4 - Query: { "age" : { "$lt" : 40} , "$and" : [ { "age" : { "$gt" : 10}}]}, Fields: null, Sort: null
userTest4 - User [id=id, ic=1002, name=bird, age=20]
userTest4 - User [id=id, ic=1003, name=cat, age=30]

Case 5 - find list and sorting example
query5 - Query: { "age" : { "$gte" : 30}}, Fields: null, Sort: { "age" : -1}
userTest5 - User [id=id, ic=1006, name=frog, age=60]
userTest5 - User [id=id, ic=1005, name=elephant, age=50]
userTest5 - User [id=id, ic=1004, name=dog, age=40]
userTest5 - User [id=id, ic=1003, name=cat, age=30]

Case 6 - find by regex example
query6 - Query: { "name" : { "$regex" : "D.*G" , "$options" : "i"}}, Fields: null, Sort: null
userTest6 - User [id=id, ic=1004, name=dog, age=40] 
```

## 下载源代码

Download it – [SpringMongoDB-Query-Example.zip](http://web.archive.org/web/20190220142213/http://www.mkyong.com/wp-content/uploads/2011/05/SpringMongoDB-Query-Example.zip) (25 KB)

## 参考

1.  [Spring data Mongodb-查询文档](http://web.archive.org/web/20190220142213/http://static.springsource.org/spring-data/mongodb/docs/current/reference/html/mongo.core.html#mongo.query)
2.  [Java MongoDB 查询示例/](http://web.archive.org/web/20190220142213/http://www.mkyong.com/mongodb/java-mongodb-query-document/)

[mongodb](http://web.archive.org/web/20190220142213/http://www.mkyong.com/tag/mongodb/) [query](http://web.archive.org/web/20190220142213/http://www.mkyong.com/tag/query/) [spring-data](http://web.archive.org/web/20190220142213/http://www.mkyong.com/tag/spring-data/)







