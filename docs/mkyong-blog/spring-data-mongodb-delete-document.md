# Spring Data MongoDB:删除文档

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/spring-data-mongodb-delete-document/>

在 MongoDB 的 Spring data 中，可以使用`remove()`和`findAndRemove()`从 MongoDB 中删除文档。

1.  remove()–删除单个或多个文档。
2.  删除单个文档，并返回被删除的文档。

**Common Mistake**
Don’t use `findAndRemove()` to perform a batch delete (remove multiple documents), only the first document that matches the query will be removed. Refer query4 below:

## 1.删除文档示例

查看完整的示例，了解 remove()和 findAndRemove()的用法。

```java
 package com.mkyong.core;

import java.util.ArrayList;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.data.mongodb.core.MongoOperations;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;

import com.mkyong.config.SpringMongoConfig;
import com.mkyong.model.User;

/**
 * Delete example
 * 
 * @author mkyong
 * 
 */

public class DeleteApp {

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
		User user5 = new User("1005", "elephant", 50);
		User user6 = new User("1006", "frog", 60);
		users.add(user1);
		users.add(user2);
		users.add(user3);
		users.add(user4);
		users.add(user5);
		users.add(user6);
		mongoOperation.insert(users, User.class);

		Query query1 = new Query();
		query1.addCriteria(Criteria.where("name").exists(true)
			.orOperator(
                           Criteria.where("name").is("frog"), 
                           Criteria.where("name").is("dog")
                        ));
		mongoOperation.remove(query1, User.class);

		Query query2 = new Query();
		query2.addCriteria(Criteria.where("name").is("bird"));
		User userTest2 = mongoOperation.findOne(query2, User.class);
		mongoOperation.remove(userTest2);

		// The first document that matches the query is returned and also
		// removed from the collection in the database.
		Query query3 = new Query();
		query3.addCriteria(Criteria.where("name").is("ant"));
		User userTest3 = mongoOperation.findAndRemove(query3, User.class);
		System.out.println("Deleted document : " + userTest3);

		// either cat or elephant is deleted only, 
                // common mistake, don't use for batch delete.
		/*
		  Query query4 = new Query(); 
                  query4.addCriteria(Criteria.where("name") .exists(true)
		       .orOperator(
                             Criteria.where("name").is("cat"),
		             Criteria.where("name").is("elephant")
                        )
                  );
		  mongoOperation.findAndRemove(query4, User.class);
		  System.out.println("Deleted document : " + userTest4);
		 */

		System.out.println("\nAll users : ");
		List<User> allUsers = mongoOperation.findAll(User.class);
		for (User user : allUsers) {
			System.out.println(user);
		}

		mongoOperation.dropCollection(User.class);

	}

} 
```

*输出*

```java
 Deleted document : User [id=5162e0153004c3cb0a907370, ic=1001, name=ant, age=10]

All users : 
User [id=5162e0153004c3cb0a907372, ic=1003, name=cat, age=30]
User [id=5162e0153004c3cb0a907374, ic=1005, name=elephant, age=50] 
```

 ## 下载源代码

Download it – [SpringMongoDB-Delete-Example.zip](http://web.archive.org/web/20190214015648/http://www.mkyong.com/wp-content/uploads/2011/05/SpringMongoDB-Delete-Example.zip) (24 KB) ## 参考

1.  [从 MongoDB 中保存、更新和删除文档](http://web.archive.org/web/20190214015648/http://static.springsource.org/spring-data/mongodb/docs/current/reference/html/mongo.core.html#mongo-template.save-update-remove)
2.  [Java MongoDB 删除示例/](http://web.archive.org/web/20190214015648/http://www.mkyong.com/mongodb/java-mongodb-delete-document/)

[delete](http://web.archive.org/web/20190214015648/http://www.mkyong.com/tag/delete/) [mongodb](http://web.archive.org/web/20190214015648/http://www.mkyong.com/tag/mongodb/) [spring-data](http://web.archive.org/web/20190214015648/http://www.mkyong.com/tag/spring-data/)







