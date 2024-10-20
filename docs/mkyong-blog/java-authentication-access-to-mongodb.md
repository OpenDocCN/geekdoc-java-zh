# Java MongoDB:认证示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-authentication-access-to-mongodb/>

默认情况下，MongoDB 运行在可信环境中(不需要使用用户名和密码进行身份验证)。在本教程中，我们将向您展示如何在安全模式下启动 MongoDB/启用身份验证，并连接 Java MongoDB 驱动程序。

## 1.在安全模式下启动 MongoDB

用`--auth`选项启动 MongoDB，现在，MongoDB 需要用户名和密码来执行任何数据库/集合操作。

```java
 mongod --auth 
```

后来，我们需要连接到数据库“testdb”，所以添加一个用户，以便稍后进行测试。

```java
 > use admin
> db.addUser("admin","password")
> use testdb
> db.addUser("mkyong","password") 
```

要启用 MongoDB 认证，您必须首先将一个用户添加到特殊的“admin”数据库，详细指南请参考此 [MongoDB 认证示例](http://web.archive.org/web/20190214234952/http://www.mkyong.com/mongodb/mongodb-authentication-example/)。

 ## 2.Java + MongoDB 认证示例

如果 MongoDB 是在安全模式下启动的，下面的“插入”操作将不再有效，并提示“需要登录”的错误信息。

```java
 Mongo mongo = new Mongo("localhost", 27017);
	DB db = mongo.getDB("testdb");

	DBCollection table = db.getCollection("user");

	BasicDBObject document = new BasicDBObject();
	document.put("name", "mkyong");
	table.insert(document); 
```

```java
 com.mongodb.CommandResult$CommandFailure: command failed [getlasterror]: 
	{ "serverUsed" : "localhost/127.0.0.1:27017" , "errmsg" : "need to login" , "ok" : 0.0}

	at com.mongodb.CommandResult.getException(CommandResult.java:88)
	at com.mongodb.CommandResult.throwOnError(CommandResult.java:134)
	at com.mongodb.DBTCPConnector._checkWriteError(DBTCPConnector.java:142)
	at com.mongodb.DBTCPConnector.say(DBTCPConnector.java:183)
	at com.mongodb.DBTCPConnector.say(DBTCPConnector.java:155)
	at com.mongodb.DBApiLayer$MyCollection.insert(DBApiLayer.java:270)
	at com.mongodb.DBApiLayer$MyCollection.insert(DBApiLayer.java:226)
	at com.mongodb.DBCollection.insert(DBCollection.java:75)
	at com.mongodb.DBCollection.insert(DBCollection.java:59)
	at com.mongodb.DBCollection.insert(DBCollection.java:104)
	at com.mkyong.core.App.main(App.java:40) 
```

现在，使用`db.authenticate()`来执行认证，返回值 true =成功，false =失败。

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import java.util.Date;
import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.Mongo;
import com.mongodb.MongoException;

/**
 * Java + MongoDB in Secure Mode
 * 
 */
public class JavaMongoDBAuthExample {
   public static void main(String[] args) {

    try {

	Mongo mongo = new Mongo("localhost", 27017);
	DB db = mongo.getDB("testdb");

	boolean auth = db.authenticate("testdb", "password".toCharArray());
	if (auth) {

		DBCollection table = db.getCollection("user");

		BasicDBObject document = new BasicDBObject();
		document.put("name", "mkyong");
		table.insert(document);

		System.out.println("Login is successful!");
	} else {
		System.out.println("Login is failed!");
	}
	System.out.println("Done");

    } catch (UnknownHostException e) {
	e.printStackTrace();
    } catch (MongoException e) {
	e.printStackTrace();
    }
  }
} 
```

 ## 参考

1.  [Java MongoDB 认证示例](http://web.archive.org/web/20190214234952/http://www.mkyong.com/mongodb/java-authentication-access-to-mongodb/)
2.  [JIRA–db . authenticate()应该使用字符[]作为密码](http://web.archive.org/web/20190214234952/https://jira.mongodb.org/browse/JAVA-45)
3.  [MongoDB Java 认证示例](http://web.archive.org/web/20190214234952/http://docs.mongodb.org/ecosystem/tutorial/getting-started-with-java-driver/#authentication-optional)
4.  [MongoDB 安全实践和管理](http://web.archive.org/web/20190214234952/http://docs.mongodb.org/manual/administration/security/)

[authentication](http://web.archive.org/web/20190214234952/http://www.mkyong.com/tag/authentication/) [mongodb](http://web.archive.org/web/20190214234952/http://www.mkyong.com/tag/mongodb/)







