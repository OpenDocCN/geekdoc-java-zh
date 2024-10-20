# Spring @PropertySource 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring/spring-propertysources-example/>



![spring-properties-example](img/7898c19ff05e1f2ceccf4be9f1886b72.png)

在 Spring 中，您可以使用`@PropertySource`注释将您的配置具体化到一个属性文件中。在本教程中，我们将向您展示如何使用`@PropertySource`来读取属性文件，并使用`@Value`和`Environment`来显示值。

*P.S @PropertySource 从 3.1 春开始可用*

## 1.@PropertySource 和@Value

一个经典的例子，读取一个属性文件，用`${}`显示。

config.properties

```java
 mongodb.url=1.2.3.4
mongodb.db=hello 
```

AppConfigMongoDB

```java
 package com.mkyong.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;
//...

@Configuration
@ComponentScan(basePackages = { "com.mkyong.*" })
@PropertySource("classpath:config.properties")
public class AppConfigMongoDB {

	//1.2.3.4
	@Value("${mongodb.url}")
	private String mongodbUrl;

	//hello
	@Value("${mongodb.db}")
	private String defaultDb;

	@Bean
	public MongoTemplate mongoTemplate() throws Exception {

		MongoClientOptions mongoOptions = 
			new MongoClientOptions.Builder().maxWaitTime(1000 * 60 * 5).build();
		MongoClient mongo = new MongoClient(mongodbUrl, mongoOptions);
		MongoDbFactory mongoDbFactory = new SimpleMongoDbFactory(mongo, defaultDb);
		return new MongoTemplate(mongoDbFactory);

	}

	//To resolve ${} in @Value
	@Bean
	public static PropertySourcesPlaceholderConfigurer propertyConfigInDev() {
		return new PropertySourcesPlaceholderConfigurer();
	}

} 
```

**Note**
To resolve ${} in `@Values`, you must register a static `PropertySourcesPlaceholderConfigurer` in either XML or annotation configuration file.freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 2.@属性源和环境

Spring 建议使用`Environment`来获取属性值。

AppConfigMongoDB

```java
 package com.mkyong.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.env.Environment;
//...

@Configuration
@ComponentScan(basePackages = { "com.mkyong.*" })
@PropertySource("classpath:config.properties")
public class AppConfigMongoDB {

	@Autowired
	private Environment env;

	@Bean
	public MongoTemplate mongoTemplate() throws Exception {

		String mongodbUrl = env.getProperty("mongodb.url");
		String defaultDb = env.getProperty("mongodb.db");

		MongoClientOptions mongoOptions = 
			new MongoClientOptions.Builder().maxWaitTime(1000 * 60 * 5).build();
		MongoClient mongo = new MongoClient(mongodbUrl, mongoOptions);
		MongoDbFactory mongoDbFactory = new SimpleMongoDbFactory(mongo, defaultDb);
		return new MongoTemplate(mongoDbFactory);

	}

} 
```

## 3.更多@PropertySource 示例

比较常见的例子。

3.1 在`@PropertySource`资源位置内解析${}的示例。

```java
 @Configuration
	@PropertySource("file:${app.home}/app.properties")
	public class AppConfig {
		@Autowired
		Environment env;
	} 
```

启动时设置系统属性。

```java
 System.setProperty("app.home", "test");

	java -jar -Dapp.home="/home/mkyon/test" example.jar 
```

3.2 包括多个属性文件。

```java
 @Configuration
	@PropertySource({
		"classpath:config.properties",
		"classpath:db.properties" //if same key, this will 'win'
	})
	public class AppConfig {
		@Autowired
		Environment env;
	} 
```

Note
If a property key is duplicated, the last declared file will ‘win’ and override.

## 4.Spring 4 和@PropertySources

Spring 4 上的一些增强。

4.1 引入了新的`@PropertySources`来支持 Java 8 和更好的方法来包含多个属性文件。

```java
 @Configuration
	@PropertySources({
		@PropertySource("classpath:config.properties"),
		@PropertySource("classpath:db.properties")
	})
	public class AppConfig {
		//...
	} 
```

4.2 允许`@PropertySource`忽略未找到的属性文件。

```java
 @Configuration
	@PropertySource("classpath:missing.properties")
	public class AppConfig {
		//...
	} 
```

如果没有找到`missing.properties`，系统无法启动并抛出`FileNotFoundException`

```java
 Caused by: java.io.FileNotFoundException: 
		class path resource [missiong.properties] cannot be opened because it does not exist 
```

在 Spring 4 中，您可以使用`ignoreResourceNotFound`来忽略未找到的属性文件

```java
 @Configuration
	@PropertySource(value="classpath:missing.properties", ignoreResourceNotFound=true)
	public class AppConfig {
		//...
	} 
```

```java
 @PropertySources({
		@PropertySource(value = "classpath:missing.properties", ignoreResourceNotFound=true),
		@PropertySource("classpath:config.properties")
        }) 
```

完成了。

## 参考

1.  [Sprong IO–property source](http://web.archive.org/web/20210205064800/https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/annotation/PropertySource.html)
2.  [Spring IO–property sources](http://web.archive.org/web/20210205064800/https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/annotation/PropertySources.html)
3.  [弹簧 IO–配置](http://web.archive.org/web/20210205064800/https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)
4.  [Spring @Value 默认值](http://web.archive.org/web/20210205064800/http://www.mkyong.com/spring3/spring-value-default-value/)
5.  [春季支尔格:SPR-8539](http://web.archive.org/web/20210205064800/https://jira.spring.io/browse/SPR-8539)

Tags : [spring](http://web.archive.org/web/20210205064800/https://mkyong.com/tag/spring/) [spring3](http://web.archive.org/web/20210205064800/https://mkyong.com/tag/spring3/) [spring4](http://web.archive.org/web/20210205064800/https://mkyong.com/tag/spring4/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="13616">