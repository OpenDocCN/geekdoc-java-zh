# Spring @Value 默认值

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring3/spring-value-default-value/>

在本教程中，我们将向您展示如何为`@Value`设置默认值

## 1.@价值示例

要在弹簧表达式中设置默认值，使用`Elvis operator`:

```java
 #{expression?:default value} 
```

几个例子:

```java
 @Value("#{systemProperties['mongodb.port'] ?: 27017}")
	private String mongodbPort;

	@Value("#{config['mongodb.url'] ?: '127.0.0.1'}")
	private String mongodbUrl;	

	@Value("#{aBean.age ?: 21}")
	private int age; 
```

*P.S `@Value`自春季 3.0* 开始发售

 ## 2.@值和属性示例

要设置属性占位符的默认值:

```java
 ${property:default value} 
```

几个例子:

```java
 //@PropertySource("classpath:/config.properties}")
	//@Configuration

	@Value("${mongodb.url:127.0.0.1}")
	private String mongodbUrl;

	@Value("#{'${mongodb.url:172.0.0.1}'}")
	private String mongodbUrl;

	@Value("#{config['mongodb.url']?:'127.0.0.1'}")
	private String mongodbUrl; 
```

config.properties

```java
 mongodb.url=1.2.3.4
mongodb.db=hello 
```

对于“配置”bean。

```java
 <util:properties id="config" location="classpath:config.properties"/> 
```

**跟进**
必须在 XML 或注释中注册一个静态`PropertySourcesPlaceholderConfigurer` bean，以便 Spring `@Value`知道如何解释`${}`

```java
 //@PropertySource("classpath:/config.properties}")
  //@Configuration

  @Bean
  public static PropertySourcesPlaceholderConfigurer propertyConfigIn() {
	return new PropertySourcesPlaceholderConfigurer();
  } 
```

 ## 参考

1.  [春埃尔的表情](http://web.archive.org/web/20190225095833/http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html)

[spring](http://web.archive.org/web/20190225095833/http://www.mkyong.com/tag/spring/) [spring3](http://web.archive.org/web/20190225095833/http://www.mkyong.com/tag/spring3/)







