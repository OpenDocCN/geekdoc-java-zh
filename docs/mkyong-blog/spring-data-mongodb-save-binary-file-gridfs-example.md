# Spring 数据 MongoDB:保存二进制文件，GridFS 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/spring-data-mongodb-save-binary-file-gridfs-example/>



![binary-data-flow](img/2207d8035ee0b6e9b5b073c026c5042b.png)

在 MongoDB 中，可以使用 GridFS 存储二进制文件。在本教程中，我们将向您展示如何使用 Spring Data 的`GridFsTemplate`在 MongoDB 中存储/读取图像。

## 1.grid fs–保存示例(注释中的 Spring 配置)

获取一个图像文件并保存到 MongoDB 中。

SpringMongoConfig.java

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.mongodb.config.AbstractMongoConfiguration;
import org.springframework.data.mongodb.gridfs.GridFsTemplate;

import com.mongodb.Mongo;
import com.mongodb.MongoClient;

/**
 * Spring MongoDB configuration file
 * 
 */
@Configuration
public class SpringMongoConfig extends AbstractMongoConfiguration{

	@Bean
	public GridFsTemplate gridFsTemplate() throws Exception {
		return new GridFsTemplate(mongoDbFactory(), mappingMongoConverter());
	}

	@Override
	protected String getDatabaseName() {
		return "yourdb";
	}

	@Override
	@Bean
	public Mongo mongo() throws Exception {
		return new MongoClient("127.0.0.1");
	}

} 
```

GridFsAppStore.java

```java
 package com.mkyong.core;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.data.mongodb.gridfs.GridFsOperations;

import com.mkyong.config.SpringMongoConfig;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;

/**
 * GridFs example
 * 
 * @author mkyong
 * 
 */

public class GridFsAppStore {

    public static void main(String[] args) {

	ApplicationContext ctx = 
                     new AnnotationConfigApplicationContext(SpringMongoConfig.class);
	GridFsOperations gridOperations = 
                      (GridFsOperations) ctx.getBean("gridFsTemplate");

	DBObject metaData = new BasicDBObject();
	metaData.put("extra1", "anything 1");
	metaData.put("extra2", "anything 2");

	InputStream inputStream = null;
	try {
		inputStream = new FileInputStream("/Users/mkyong/Downloads/testing.png");
		gridOperations.store(inputStream, "testing.png", "image/png", metaData);

	} catch (FileNotFoundException e) {
		e.printStackTrace();
	} finally {
		if (inputStream != null) {
			try {
				inputStream.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}

		System.out.println("Done");
    }

} 
```

查看 MongoDB 控制台，看看保存了什么。

```java
 > show dbs
yourdb	0.203125GB

> use yourdb
switched to db yourdb

> show collections
fs.chunks
fs.files
system.indexes

> db.fs.files.find()
{ "_id" : ObjectId("51641c5630045c9b3db35afc"), "chunkSize" : NumberLong(262144), 
"length" : NumberLong(4238), "md5" : "9969527cd95a5a573f15e953f0036800", "filename" : "testing.png", 
"contentType" : "image/png", "uploadDate" : ISODate("2013-04-09T13:49:10.104Z"), 
"aliases" : null, "metadata" : { "extra1" : "anything 1", "extra2" : "anything 2" } }
>

> db.fs.chunks.find()
{ "_id" : ObjectId("51641c5630045c9b3db35afd"), 
"files_id" : ObjectId("51641c5630045c9b3db35afc"), "n" : 0, 
"data" : BinData(0,"/9j/4AAQSkZJRgABAgAAZ......EQH/9k=") } 
```

图像信息保存在`fs.files`中，图像文件(二进制转换)保存在`fs.chunks`中，通过`_id`和`files_id`链接。

## 2.grid fs–读取示例(XML 文件中的 Spring 配置)

从 MongoDB 中读取上面的图像文件，并将其保存为另一个图像。

SpringConfig.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:mongo="http://www.springframework.org/schema/data/mongo"
	xsi:schemaLocation="http://www.springframework.org/schema/data/mongo 
       http://www.springframework.org/schema/data/mongo/spring-mongo.xsd
       http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">

	<mongo:db-factory id="mongoDbFactory" dbname="yourdb" />
	<mongo:mapping-converter id="converter" />

	<bean name="gridFsTemplate"
		class="org.springframework.data.mongodb.gridfs.GridFsTemplate">
		<constructor-arg ref="mongoDbFactory" />
		<constructor-arg ref="converter" />
	</bean>

</beans> 
```

GridFsAppStore.java

```java
 package com.mkyong.core;

import java.io.IOException;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.gridfs.GridFsOperations;

import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.gridfs.GridFSDBFile;

/**
 * GridFs example
 * 
 * @author mkyong
 * 
 */

public class GridFsAppRead {

    public static void main(String[] args) {

	ApplicationContext ctx = 
              new GenericXmlApplicationContext("SpringConfig.xml");
	GridFsOperations gridOperations = 
              (GridFsOperations) ctx.getBean("gridFsTemplate");

	List<GridFSDBFile> result = gridOperations.find(
               new Query().addCriteria(Criteria.where("filename").is("testing.png")));

	for (GridFSDBFile file : result) {
		try {
			System.out.println(file.getFilename());
			System.out.println(file.getContentType());

			//save as another image
			file.writeTo("/Users/mkyong/Downloads/new-testing.png");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	System.out.println("Done");

    }
} 
```

## 下载源代码

Download it – [SpringMongoDB-GridFs-Example.zip](http://web.archive.org/web/20200123144222/http://www.mkyong.com/wp-content/uploads/2013/04/SpringMongoDB-GridFs-Example.zip) (24 KB)

## 参考

1.  [AbstractMongoConfiguration JavaDoc](http://web.archive.org/web/20200123144222/http://static.springsource.org/spring-data/data-document/docs/current/api/org/springframework/data/mongodb/config/AbstractMongoConfiguration.html)
2.  [Spring Data MongoDB GridFS 支持](http://web.archive.org/web/20200123144222/http://static.springsource.org/spring-data/mongodb/docs/current/reference/html/mongo.core.html#gridfs)
3.  [春季数据 MongoDB Hello World 示例](http://web.archive.org/web/20200123144222/http://www.mkyong.com/mongodb/spring-data-mongodb-hello-world-example/)
4.  [Java MongoDB:保存图像示例](http://web.archive.org/web/20200123144222/http://www.mkyong.com/mongodb/java-mongodb-save-image-example/)

[binary data](http://web.archive.org/web/20200123144222/https://mkyong.com/tag/binary-data/) [gridfs](http://web.archive.org/web/20200123144222/https://mkyong.com/tag/gridfs/) [mongodb](http://web.archive.org/web/20200123144222/https://mkyong.com/tag/mongodb/) [spring-data](http://web.archive.org/web/20200123144222/https://mkyong.com/tag/spring-data/)<input type="hidden" id="mkyong-current-postId" value="12931">









