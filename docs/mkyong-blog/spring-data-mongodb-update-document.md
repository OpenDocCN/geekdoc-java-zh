# Spring 数据 MongoDB:更新文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/spring-data-mongodb-update-document/>

在 Spring data-MongoDB 中，您可以使用以下方法来更新文档。

1.  保存–更新整个对象，如果“_id”存在，则执行更新，否则插入它。
2.  update first–更新匹配查询的第一个文档。
3.  update multi–更新与查询匹配的所有文档。
4.  upserting–如果没有与查询匹配的文档，则通过组合查询和更新对象来创建新文档。
5.  findAndModify–与 updateMulti 相同，但是它有一个额外的选项，可以返回旧的或新更新的文档。

*注:所有例子都在`mongo-java-driver-2.11.0.jar`和`spring-data-mongodb-1.2.0.RELEASE.jar`和*下测试

## 1.saveourupdate–第 1 部分示例

假设下面 json 数据被插入 MongoDB。

```java
 { 
	"_id" : ObjectId("id"), 
	"ic" : "1001", 
	"name" : "appleA", 
	"age" : 20, 
	"createdDate" : ISODate("2013-04-06T23:17:35.530Z") 
} 
```

找到文档，用`save()`方法修改更新。

```java
 Query query = new Query();
	query.addCriteria(Criteria.where("name").is("appleA"));

	User userTest1 = mongoOperation.findOne(query, User.class);

	System.out.println("userTest1 - " + userTest1);

	//modify and update with save()
	userTest1.setAge(99);
	mongoOperation.save(userTest1);

	//get the updated object again
	User userTest1_1 = mongoOperation.findOne(query, User.class);

	System.out.println("userTest1_1 - " + userTest1_1); 
```

输出

```java
 userTest1 - User [id=id, ic=1001, name=appleA, age=20, createdDate=Sat Apr 06 23:17:35 MYT 2013]
userTest1_1 - User [id=id, ic=1001, name=appleA, age=99, createdDate=Sat Apr 06 23:17:35 MYT 2013] 
```

**Note**
See example 2, it shows a common mistake made by most of the developers. ## 2.saveourupdate–第 2 部分示例

这是一个失败的例子，仔细阅读，一个真正常见的错误。

假设下面 json 数据被插入 MongoDB。

```java
 { 
	"_id" : ObjectId("id"), 
	"ic" : "1002", 
	"name" : "appleB", 
	"age" : 20, 
	"createdDate" : ISODate("2013-04-06T15:22:34.530Z") 
} 
```

在`Query`中，您得到的文档只返回了一个“name”字段值，这确实经常发生，以保存对象返回的大小。返回的“用户”对象在以下字段中具有空值:年龄、ic 和创建日期，如果您修改“年龄”字段并更新它，它将覆盖所有内容，而不是更新修改后的字段-“年龄”。

```java
 Query query = new Query();
		query.addCriteria(Criteria.where("name").is("appleB"));
		query.fields().include("name");

		User userTest2 = mongoOperation.findOne(query, User.class);
		System.out.println("userTest2 - " + userTest2);

		userTest2.setAge(99);

		mongoOperation.save(userTest2);

		// ooppss, you just override everything, it caused ic=null and
		// createdDate=null

		Query query1 = new Query();
		query1.addCriteria(Criteria.where("name").is("appleB"));

		User userTest2_1 = mongoOperation.findOne(query1, User.class);
		System.out.println("userTest2_1 - " + userTest2_1); 
```

输出

```java
 userTest2 - User [id=51603dba3004d7fffc202391, ic=null, name=appleB, age=0, createdDate=null]
userTest2_1 - User [id=51603dba3004d7fffc202391, ic=null, name=appleB, age=99, createdDate=null] 
```

在 save()之后，字段“age”被正确更新，但是 ic 和 createdDate 都被设置为 null，整个“user”对象被更新。若要更新单个字段/键值，请不要使用 save()，而是使用 updateFirst()或 updateMulti()。

 ## 3.updateFirst 示例

更新匹配查询的第一个文档。在这种情况下，只有单个字段“年龄”被更新。

```java
 { 
	"_id" : ObjectId("id"), 
	"ic" : "1003", 
	"name" : "appleC", 
	"age" : 20, 
	"createdDate" : ISODate("2013-04-06T23:22:34.530Z") 
} 
```

```java
 //returns only 'name' field
		Query query = new Query();
		query.addCriteria(Criteria.where("name").is("appleC"));
		query.fields().include("name");

		User userTest3 = mongoOperation.findOne(query, User.class);
		System.out.println("userTest3 - " + userTest3);

		Update update = new Update();
		update.set("age", 100);

		mongoOperation.updateFirst(query, update, User.class);

		//returns everything
		Query query1 = new Query();
		query1.addCriteria(Criteria.where("name").is("appleC"));

		User userTest3_1 = mongoOperation.findOne(query1, User.class);
		System.out.println("userTest3_1 - " + userTest3_1); 
```

输出

```java
 userTest3 - User [id=id, ic=null, name=appleC, age=0, createdDate=null]
userTest3_1 - User [id=id, ic=1003, name=appleC, age=100, createdDate=Sat Apr 06 23:22:34 MYT 2013] 
```

## 4.updateMulti 示例

更新与查询匹配的所有文档。

```java
 { 
	"_id" : ObjectId("id"), 
	"ic" : "1004", 
	"name" : "appleD", 
	"age" : 20, 
	"createdDate" : ISODate("2013-04-06T15:22:34.530Z") 
}
{ 
	"_id" : ObjectId("id"), 
	"ic" : "1005", 
	"name" : "appleE", 
	"age" : 20, 
	"createdDate" : ISODate("2013-04-06T15:22:34.530Z") 
} 
```

```java
 //show the use of $or operator
		Query query = new Query();
		query.addCriteria(Criteria
				.where("name").exists(true)
				.orOperator(Criteria.where("name").is("appleD"),
						Criteria.where("name").is("appleE")));
		Update update = new Update();

		//update age to 11
		update.set("age", 11);

		//remove the createdDate field
		update.unset("createdDate");

		// if use updateFirst, it will update 1004 only.
		// mongoOperation.updateFirst(query4, update4, User.class);

		// update all matched, both 1004 and 1005
		mongoOperation.updateMulti(query, update, User.class);

		System.out.println(query.toString());

		List<User> usersTest4 = mongoOperation.find(query4, User.class);

		for (User userTest4 : usersTest4) {
			System.out.println("userTest4 - " + userTest4);
		} 
```

输出

```java
 Query: { "name" : { "$exists" : true} , 
	"$or" : [ { "name" : "appleD"} , { "name" : "appleE"}]}, Fields: null, Sort: null

userTest4 - User [id=id, ic=1004, name=appleD, age=11, createdDate=null]
userTest4 - User [id=id, ic=1005, name=appleE, age=11, createdDate=null] 
```

## 5.向上插入示例

如果文档匹配，则更新它，否则通过组合查询和更新对象创建一个新文档，其工作方式类似于`findAndModifyElseCreate()`:)

```java
 { 
	//no data
} 
```

```java
 //search a document that doesn't exist
		Query query = new Query();
		query.addCriteria(Criteria.where("name").is("appleZ"));

		Update update = new Update();
		update.set("age", 21);

		mongoOperation.upsert(query, update, User.class);

		User userTest5 = mongoOperation.findOne(query, User.class);
		System.out.println("userTest5 - " + userTest5); 
```

输出，通过组合查询和更新对象创建一个新文档。

```java
 userTest5 - User [id=id, ic=null, name=appleZ, age=21, createdDate=null] 
```

## 6.findAndModify 示例

从单个操作中查找、修改并获取新更新的对象。

```java
 { 
	"_id" : ObjectId("id"), 
	"ic" : "1006", 
	"name" : "appleF", 
	"age" : 20, 
	"createdDate" : ISODate("2013-04-07T13:11:34.530Z") 
} 
```

```java
 Query query6 = new Query();
		query6.addCriteria(Criteria.where("name").is("appleF"));

		Update update6 = new Update();
		update6.set("age", 101);
		update6.set("ic", 1111);

		//FindAndModifyOptions().returnNew(true) = newly updated document
		//FindAndModifyOptions().returnNew(false) = old document (not update yet)
		User userTest6 = mongoOperation.findAndModify(
				query6, update6, 
				new FindAndModifyOptions().returnNew(true), User.class);
		System.out.println("userTest6 - " + userTest6); 
```

输出

```java
 userTest6 - User [id=id, ic=1111, name=appleF, age=101, createdDate=Sun Apr 07 13:11:34 MYT 2013] 
```

## 7.完整示例

完全适用于将例 1 到例 6 中的所有内容结合起来。

```java
 package com.mkyong.core;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.data.mongodb.core.FindAndModifyOptions;
import org.springframework.data.mongodb.core.MongoOperations;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Update;

import com.mkyong.config.SpringMongoConfig;
import com.mkyong.model.User;

public class UpdateApp {

	public static void main(String[] args) {
		// For Annotation
		ApplicationContext ctx = 
			new AnnotationConfigApplicationContext(SpringMongoConfig.class);
		MongoOperations mongoOperation = 
			(MongoOperations) ctx.getBean("mongoTemplate");

		// insert 6 users for testing
		List<User> users = new ArrayList<User>();

		User user1 = new User("1001", "appleA", 20, new Date());
		User user2 = new User("1002", "appleB", 20, new Date());
		User user3 = new User("1003", "appleC", 20, new Date());
		User user4 = new User("1004", "appleD", 20, new Date());
		User user5 = new User("1005", "appleE", 20, new Date());
		User user6 = new User("1006", "appleF", 20, new Date());
		users.add(user1);
		users.add(user2);
		users.add(user3);
		users.add(user4);
		users.add(user5);
		users.add(user6);
		mongoOperation.insert(users, User.class);

		// Case 1 ... find and update
		System.out.println("Case 1");

		Query query1 = new Query();
		query1.addCriteria(Criteria.where("name").is("appleA"));

		User userTest1 = mongoOperation.findOne(query1, User.class);

		System.out.println("userTest1 - " + userTest1);

		userTest1.setAge(99);
		mongoOperation.save(userTest1);

		User userTest1_1 = mongoOperation.findOne(query1, User.class);

		System.out.println("userTest1_1 - " + userTest1_1);

		// Case 2 ... select single field only
		System.out.println("\nCase 2");

		Query query2 = new Query();
		query2.addCriteria(Criteria.where("name").is("appleB"));
		query2.fields().include("name");

		User userTest2 = mongoOperation.findOne(query2, User.class);
		System.out.println("userTest2 - " + userTest2);

		userTest2.setAge(99);

		mongoOperation.save(userTest2);

		// ooppss, you just override everything, it caused ic=null and
		// createdDate=null

		Query query2_1 = new Query();
		query2_1.addCriteria(Criteria.where("name").is("appleB"));

		User userTest2_1 = mongoOperation.findOne(query2_1, User.class);
		System.out.println("userTest2_1 - " + userTest2_1);

		System.out.println("\nCase 3");
		Query query3 = new Query();
		query3.addCriteria(Criteria.where("name").is("appleC"));
		query3.fields().include("name");

		User userTest3 = mongoOperation.findOne(query3, User.class);
		System.out.println("userTest3 - " + userTest3);

		Update update3 = new Update();
		update3.set("age", 100);

		mongoOperation.updateFirst(query3, update3, User.class);

		Query query3_1 = new Query();
		query3_1.addCriteria(Criteria.where("name").is("appleC"));

		User userTest3_1 = mongoOperation.findOne(query3_1, User.class);
		System.out.println("userTest3_1 - " + userTest3_1);

		System.out.println("\nCase 4");
		Query query4 = new Query();
		query4.addCriteria(Criteria
				.where("name")
				.exists(true)
				.orOperator(Criteria.where("name").is("appleD"),
						Criteria.where("name").is("appleE")));
		Update update4 = new Update();
		update4.set("age", 11);
		update4.unset("createdDate");

		// update 1004 only.
		// mongoOperation.updateFirst(query4, update4, User.class);

		// update all matched
		mongoOperation.updateMulti(query4, update4, User.class);

		System.out.println(query4.toString());

		List<User> usersTest4 = mongoOperation.find(query4, User.class);

		for (User userTest4 : usersTest4) {
			System.out.println("userTest4 - " + userTest4);
		}

		System.out.println("\nCase 5");
		Query query5 = new Query();
		query5.addCriteria(Criteria.where("name").is("appleZ"));

		Update update5 = new Update();
		update5.set("age", 21);

		mongoOperation.upsert(query5, update5, User.class);

		User userTest5 = mongoOperation.findOne(query5, User.class);
		System.out.println("userTest5 - " + userTest5);

		System.out.println("\nCase 6");
		Query query6 = new Query();
		query6.addCriteria(Criteria.where("name").is("appleF"));

		Update update6 = new Update();
		update6.set("age", 101);
		update6.set("ic", 1111);

		User userTest6 = mongoOperation.findAndModify(query6, update6,
				new FindAndModifyOptions().returnNew(true), User.class);
		System.out.println("userTest6 - " + userTest6);

		mongoOperation.dropCollection(User.class);

	}

} 
```

输出

```java
 Case 1
userTest1 - User [id=id, ic=1001, name=appleA, age=20, createdDate=Sun Apr 07 13:22:48 MYT 2013]
userTest1_1 - User [id=id, ic=1001, name=appleA, age=99, createdDate=Sun Apr 07 13:22:48 MYT 2013]

Case 2
userTest2 - User [id=id, ic=null, name=appleB, age=0, createdDate=null]
userTest2_1 - User [id=id, ic=null, name=appleB, age=99, createdDate=null]

Case 3
userTest3 - User [id=id, ic=null, name=appleC, age=0, createdDate=null]
userTest3_1 - User [id=id, ic=1003, name=appleC, age=100, createdDate=Sun Apr 07 13:22:48 MYT 2013]

Case 4
Query: { "name" : { "$exists" : true} , "$or" : [ { "name" : "appleD"} , { "name" : "appleE"}]}, Fields: null, Sort: null
userTest4 - User [id=id, ic=1004, name=appleD, age=11, createdDate=null]
userTest4 - User [id=id, ic=1005, name=appleE, age=11, createdDate=null]

Case 5
userTest5 - User [id=id, ic=null, name=appleZ, age=21, createdDate=null]

Case 6
userTest6 - User [id=id, ic=1006, name=appleF, age=20, createdDate=Sun Apr 07 13:22:48 MYT 2013] 
```

## 下载源代码

Download it – [SpringMongoDB-Update-Example.zip](http://web.archive.org/web/20190305234740/http://www.mkyong.com/wp-content/uploads/2011/05/SpringMongoDB-Update-Example.zip) (29 KB)

## 参考

1.  [MongoDB 模板更新文档](http://web.archive.org/web/20190305234740/http://static.springsource.org/spring-data/data-document/docs/current/reference/html/#mongodb-template-update)
2.  [Spring Data MongoDB–保存、更新和删除文档](http://web.archive.org/web/20190305234740/http://static.springsource.org/spring-data/mongodb/docs/current/reference/html/mongo.core.html#mongo-template.save-update-remove)
3.  [Java MongoDB 更新示例/](http://web.archive.org/web/20190305234740/http://www.mkyong.com/mongodb/java-mongodb-update-document/)
4.  [MongoDB 更新修改器操作](http://web.archive.org/web/20190305234740/http://docs.mongodb.org/manual/core/update/)
5.  [春季数据 MongoDB Hello World 示例](http://web.archive.org/web/20190305234740/http://www.mkyong.com/mongodb/spring-data-mongodb-hello-world-example/)

[mongodb](http://web.archive.org/web/20190305234740/http://www.mkyong.com/tag/mongodb/) [spring-data](http://web.archive.org/web/20190305234740/http://www.mkyong.com/tag/spring-data/) [update](http://web.archive.org/web/20190305234740/http://www.mkyong.com/tag/update/)







