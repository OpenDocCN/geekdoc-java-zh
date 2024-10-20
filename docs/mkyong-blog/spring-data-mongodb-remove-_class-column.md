# Spring 数据 MongoDB remove _class 列

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/spring-data-mongodb-remove-_class-column/>

默认情况下，SpringData 的`MappingMongoConverter`为 MongoDB 中保存的每个对象添加一个额外的“ **_class** ”列。举个例子，

```java
 public class User {

	String username;
	String password;

	//...getters and setters
} 
```

保存

```java
 MongoOperations mongoOperation = (MongoOperations)ctx.getBean("mongoTemplate");	
    User user = new User("mkyong", "password123");
    mongoOperation.save(user, "users"); 
```

结果

```java
 > db.users.find()
{ 
	"_class" : "com.mkyong.user.User", 
	"_id" : ObjectId("5050aef830041f24ff2bd16e"), 
	"password" : "new password", "username" : "mkyong" 
} 
```

由于一个[原因](http://web.archive.org/web/20190214223759/http://stackoverflow.com/questions/6810488/spring-data-mongodb-mappingmongoconverter-remove-class/)，SpringData 创建了这个额外的 **_class** 。要删除这个额外的“**_ 类**，覆盖`MappingMongoConverter`，并传递一个`new DefaultMongoTypeMapper(null)`。

在这里，我们向您展示如何以注释或 XML 的方式删除 _class。

## 1.注释

```java
 @Configuration
public class SpringMongoConfig{

  public @Bean
  MongoDbFactory mongoDbFactory() throws Exception {
	return new SimpleMongoDbFactory(new Mongo(), "database");
  }

  public @Bean
  MongoTemplate mongoTemplate() throws Exception {

	//remove _class
	MappingMongoConverter converter = 
		new MappingMongoConverter(mongoDbFactory(), new MongoMappingContext());
	converter.setTypeMapper(new DefaultMongoTypeMapper(null));

	MongoTemplate mongoTemplate = new MongoTemplate(mongoDbFactory(), converter);

	return mongoTemplate;

  }

} 
```

 ## 2.可扩展标记语言

同样的事情，但是在 XML 文件中。

```java
 <mongo:mongo host="localhost" port="27017" />
<mongo:db-factory dbname="database" />

 <bean id="mappingContext"
	class="org.springframework.data.mongodb.core.mapping.MongoMappingContext" />

 <bean id="defaultMongoTypeMapper"
	class="org.springframework.data.mongodb.core.convert.DefaultMongoTypeMapper">
	<constructor-arg name="typeKey"><null/></constructor-arg>
 </bean>

 <bean id="mappingMongoConverter"
	class="org.springframework.data.mongodb.core.convert.MappingMongoConverter">
	<constructor-arg name="mongoDbFactory" ref="mongoDbFactory" />
	<constructor-arg name="mappingContext" ref="mappingContext" />
	<property name="typeMapper" ref="defaultMongoTypeMapper" />
 </bean>

 <bean id="mongoTemplate" class="org.springframework.data.mongodb.core.MongoTemplate">
	<constructor-arg name="mongoDbFactory" ref="mongoDbFactory" />
	<constructor-arg name="mongoConverter" ref="mappingMongoConverter" />
 </bean> 
```

 ## 3.再测试一次

再保存一次，“_class”就没了。

```java
 > db.users.find()
{ 
	"_id" : ObjectId("random code"), 
	"password" : "new password", "username" : "mkyong" 
} 
```

## 参考

1.  [stack overflow–mapping mongo converter remove _ class](http://web.archive.org/web/20190214223759/http://stackoverflow.com/questions/6810488/spring-data-mongodb-mappingmongoconverter-remove-class/)
2.  [春季论坛–mapping mongoconverter remove _ class](http://web.archive.org/web/20190214223759/http://forum.springsource.org/showthread.php?112505-Spring-data-MongoDb-MappingMongoConverter-remove-_class)

[mongodb](http://web.archive.org/web/20190214223759/http://www.mkyong.com/tag/mongodb/) [spring-data](http://web.archive.org/web/20190214223759/http://www.mkyong.com/tag/spring-data/)







