# 由构造者进行弹簧自动布线

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-autowiring-by-constructor/>

在 Spring 中，“**由构造器**自动连线”实际上是[由构造器参数中的类型](http://web.archive.org/web/20190216035626/http://www.mkyong.com/spring/spring-autowiring-by-type/)自动连线。这意味着，如果一个 bean 的数据类型与另一个 bean 构造函数参数的数据类型相同，就自动连接它。

查看构造函数的 Spring 自动布线的完整示例。

## 1.豆子

两个豆子，开发者和语言。

```java
 package com.mkyong.common;

public class Developer {
	private Language language;

	//autowire by constructor
	public Developer(Language language) {
		this.language = language;
	}

	//...

} 
```

```java
 package com.mkyong.common;

public class Language {
	private String name;
	//...
} 
```

 ## 2.弹簧布线

通常，通过构造函数连接 bean，如下所示:

```java
 <bean id="developer" class="com.mkyong.common.Developer">
		<constructor-arg>
			<ref bean="language" />
		</constructor-arg>
	</bean>

	<bean id="language" class="com.mkyong.common.Language" >
		<property name="name" value="Java" />
	</bean> 
```

*输出*

```java
 Developer [language=Language [name=Java]] 
```

启用**通过构造器自动连线**后，可以不设置构造器属性。Spring 会找到兼容的数据类型并自动连接它。

```java
 <bean id="developer" class="com.mkyong.common.Developer" autowire="constructor" />

	<bean id="language" class="com.mkyong.common.Language" >
		<property name="name" value="Java" />
	</bean> 
```

*输出*

```java
 Developer [language=Language [name=Java]] 
```

 ## 下载源代码

Download it – [Spring-AutoWiring-by-Constructor-Example.zip](http://web.archive.org/web/20190216035626/http://www.mkyong.com/wp-content/uploads/2011/06/Spring-AutoWiring-by-Constructor-Example.zip) (6 KB)

## 参考

1.  [Spring DI 通过构造器](http://web.archive.org/web/20190216035626/http://www.mkyong.com/spring/spring-di-via-constructor/)
2.  [弹簧按类型自动布线](http://web.archive.org/web/20190216035626/http://www.mkyong.com/spring/spring-autowiring-by-type/)

[spring](http://web.archive.org/web/20190216035626/http://www.mkyong.com/tag/spring/) [wiring](http://web.archive.org/web/20190216035626/http://www.mkyong.com/tag/wiring/)







