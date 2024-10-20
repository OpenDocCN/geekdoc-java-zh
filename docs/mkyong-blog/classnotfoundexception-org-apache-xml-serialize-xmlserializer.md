# ClassNotFoundException:org . Apache . XML . serialize . XML serializer

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/classnotfoundexception-org-apache-xml-serialize-xmlserializer/>

## 问题

使用 **Spring OXM + Castor 绑定**，添加了 Castor 库，但是仍然会出现下面的错误消息？

```java
 Exception in thread "main" java.lang.RuntimeException: 
	Could not instantiate serializer org.apache.xml.serialize.XMLSerializer: 
	java.lang.ClassNotFoundException: org.apache.xml.serialize.XMLSerializer
	at org.exolab.castor.xml.XercesSerializer.<init>(XercesSerializer.java:50)
	//... 
```

Maven 中的 Castor 依赖。

```java
 <dependency>
		<groupId>org.codehaus.castor</groupId>
		<artifactId>castor</artifactId>
		<version>1.2</version>
	</dependency>
```

## 解决办法

如果没有弄错的话， **Castor 需要 Xerces 来工作**，所以，您还需要添加 Xerces 依赖项。

```java
 <dependencies>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-oxm</artifactId>
			<version>3.0.5.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.codehaus.castor</groupId>
			<artifactId>castor</artifactId>
			<version>1.2</version>
		</dependency>

		<dependency>
			<groupId>xerces</groupId>
			<artifactId>xercesImpl</artifactId>
			<version>2.8.1</version>
		</dependency>

	</dependencies> 
```

Tags : [oxm](http://web.archive.org/web/20200616172123/https://mkyong.com/tag/oxm/) [spring3](http://web.archive.org/web/20200616172123/https://mkyong.com/tag/spring3/)<input type="hidden" id="mkyong-current-postId" value="9378">