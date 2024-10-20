# spring 3 MVC ContentNegotiatingViewResolver 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-3-mvc-contentnegotiatingviewresolver-example/>

Spring 3，**contentnegotiatingviewrolver**，是一个有趣的视图解析器，它允许你输出相同的资源(内容或数据)到不同类型的视图，如 **JSP** 、 **XML** 、 **RSS** 、 **JSON** 等。简单地说，看下面的 web 请求的 URL，它将在不同的视图中返回。

1.  `http://www.mkyong.com/fruit/banana.rss`，作为 RSS 文件返回。
2.  `http://www.mkyong.com/fruit/banana.xml`，以 XML 文件形式返回。
3.  `http://www.mkyong.com/fruit/banana.json`，作为 JSON 文件返回。
4.  `http://www.mkyong.com/fruit/banana`，返回到您的默认视图解析器。

**Note**
This **ContentNegotiatingViewResolver** first determine “which view resolver should return by file extension”, if no view is match, then use the default view resolver. Read this [Spring documentation](http://web.archive.org/web/20190213135405/http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/web/servlet/view/ContentNegotiatingViewResolver.html) to study how it works.

在本教程中，我们将向您展示如何使用`ContentNegotiatingViewResolver`。在本教程结束时，根据请求的文件扩展名，相同的模型将以不同的视图返回——XML、JSON、RSS 和 JSP。

使用的技术:

1.  弹簧 3.0.5 释放
2.  杰克逊 1.7.1
3.  罗马 1.0.0
4.  JDK 1.6
5.  maven3
6.  Eclipse 3.6

**Note**
JAXB is bundled in JDK1.6, so, you don’t need to include it manually.

## 1.项目依赖性

在 Maven `pom.xml`文件中声明以下依赖关系。

```java
 <properties>
		<spring.version>3.0.5.RELEASE</spring.version>
	</properties>

	<dependencies>

		<!-- Spring 3 dependencies -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- Jackson JSON Mapper -->
		<dependency>
			<groupId>org.codehaus.jackson</groupId>
			<artifactId>jackson-mapper-asl</artifactId>
			<version>1.7.1</version>
		</dependency>

		<!-- RSS -->
		<dependency>
			<groupId>net.java.dev.rome</groupId>
			<artifactId>rome</artifactId>
			<version>1.0.0</version>
		</dependency>

	</dependencies>

</project> 
```

 ## 2.模型

一个 Pojo，用 JAXB 注释进行了注释，因此它可以在 XML 文件中输出。除此之外，以后我们用这个模型来显示不同的视图。

```java
 package com.mkyong.common.model;

import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name = "fruit")
public class Fruit {

	String name;
	int quality;

	public String getName() {
		return name;
	}

	@XmlElement
	public void setName(String name) {
		this.name = name;
	}

	public int getQuality() {
		return quality;
	}

	@XmlElement
	public void setQuality(int quality) {
		this.quality = quality;
	}

	public Fruit(String name, int quality) {
		this.name = name;
		this.quality = quality;
	}

	public Fruit() {
	}

} 
```

 ## 3.JSON 和 XML 视图

要输出 JSON 和 XML 视图，你不需要做任何额外的工作，Spring MVC 会自动处理转换。阅读这个 [Spring MVC 和 XML](http://web.archive.org/web/20190213135405/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-xml-example/) ，以及 [Spring MVC 和 JSON](http://web.archive.org/web/20190213135405/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-json-example/) 的例子。

## 4.RSS 视图

要输出 RSS 视图，需要扩展`AbstractRssFeedView`。阅读这个 [Spring 3 MVC 和 RSS 示例](http://web.archive.org/web/20190213135405/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-rss-feed-example/)来了解它是如何工作的。

```java
 package com.mkyong.common.rss;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.view.feed.AbstractRssFeedView;
import com.mkyong.common.model.Fruit;
import com.sun.syndication.feed.rss.Channel;
import com.sun.syndication.feed.rss.Content;
import com.sun.syndication.feed.rss.Item;

public class RssFeedView extends AbstractRssFeedView {

	@Override
	protected void buildFeedMetadata(Map<String, Object> model, Channel feed,
		HttpServletRequest request) {

		feed.setTitle("Sample Title");
		feed.setDescription("Sample Description");
		feed.setLink("http://google.com");

		super.buildFeedMetadata(model, feed, request);
	}

	@Override
	protected List<Item> buildFeedItems(Map<String, Object> model,
			HttpServletRequest request, HttpServletResponse response)
			throws Exception {

		Fruit fruit = (Fruit) model.get("model");
		String msg = fruit.getName() + fruit.getQuality();

		List<Item> items = new ArrayList<Item>(1);
		Item item = new Item();
		item.setAuthor("mkyong");
		item.setLink("http://www.mkyong.com");

		Content content = new Content();
		content.setValue(msg);

		item.setContent(content);

		items.add(item);

		return items;
	}
} 
```

## 5.JSP 视图

显示模型数据的 JSP 页面。

*文件:list.jsp*

```java
 <html>
<body>
	<h1>Spring @MVC ContentNegotiatingViewResolver</h1>

	Fruit Name : ${model.name} <br />
	Fruit Quality : ${model.quality}

</body>
</html> 
```

## 6.控制器

Spring 控制器，生成一个“水果”模型并返回它。

```java
 package com.mkyong.common.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import com.mkyong.common.model.Fruit;

@Controller
@RequestMapping("/fruit")
public class FruitController {

	@RequestMapping(value="{fruitName}", method = RequestMethod.GET)
	public String getFruit(@PathVariable String fruitName, ModelMap model) {

		Fruit fruit = new Fruit(fruitName, 1000);
		model.addAttribute("model", fruit);

		return "list";

	}

} 
```

## 7.ContentNegotiatingViewResolver 示例

代码应该是不言自明的。但是，您必须定义" **order** 属性，其中较低的值获得较高的优先级。在这种情况下，当请求一个 URL 时，Spring MVC 将使用“`ContentNegotiatingViewResolver`”(order = 1)返回一个合适的视图(基于“mediaTypes”属性中声明的文件扩展名)，如果不匹配，则使用“`InternalResourceViewResolver`”(order = 2)返回一个默认的 JSP 页面。

```java
 <beans 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-3.0.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

	<context:component-scan base-package="com.mkyong.common.controller" />

	<mvc:annotation-driven />

	<bean class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
	  <property name="order" value="1" />
	  <property name="mediaTypes">
		<map>
		   <entry key="json" value="application/json" />
		   <entry key="xml" value="application/xml" />
		   <entry key="rss" value="application/rss+xml" />
		</map>
	  </property>

	  <property name="defaultViews">
		<list>
		  <!-- JSON View -->
		  <bean
			class="org.springframework.web.servlet.view.json.MappingJacksonJsonView">
		  </bean>

		  <!-- RSS View -->
		  <bean class="com.mkyong.common.rss.RssFeedView" />

		  <!-- JAXB XML View -->
		  <bean class="org.springframework.web.servlet.view.xml.MarshallingView">
			<constructor-arg>
				<bean class="org.springframework.oxm.jaxb.Jaxb2Marshaller">
				   <property name="classesToBeBound">
					<list>
					   <value>com.mkyong.common.model.Fruit</value>
					</list>
				   </property>
				</bean>
			</constructor-arg>
		  </bean>
		 </list>
	  </property>
	  <property name="ignoreAcceptHeader" value="true" />

	</bean>

	<!-- If no extension matched, use JSP view -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="order" value="2" />
		<property name="prefix">
			<value>/WEB-INF/pages/</value>
		</property>
		<property name="suffix">
			<value>.jsp</value>
		</property>
	</bean>

</beans> 
```

## 8.演示

通过`ContentNegotiatingViewResolver`在不同的视图中显示相同的型号。

*http://localhost:8080/spring MVC/fruit/banana . xml*，显示为 XML 文件。

![spring mvc and xml demo](img/6e1eac66aa0707cf89a550aa21e78a6e.png "spring-mvc-contentneg-xml")

*http://localhost:8080/spring MVC/fruit/banana . json*，显示为 JSON 文件。

![spring mvc and json demo](img/99d29d8174ddbef08f68b34056265739.png "spring-mvc-contentneg-json")

*http://localhost:8080/spring MVC/fruit/banana . rss*，显示为 RSS 文件。

![spring mvc and RSS demo](img/214ba6d6696be51df3583e001f43b5cf.png "spring-mvc-contentneg-rss")

*http://localhost:8080/spring MVC/fruit/banana*，显示为 JSP 页面。

![spring mvc and JSP demo](img/428998a2924526cb3af85fbffb938d05.png "spring-mvc-contentneg-jsp")

## 下载源代码

Download it – [SpringMVC-ContentNegotiatingViewResolver-Example.zip](http://web.archive.org/web/20190213135405/http://www.mkyong.com/wp-content/uploads/2011/08/SpringMVC-ContentNegotiatingViewResolver-Example.zip) (10 KB)

## 参考

*   [Spring ContentNegotiatingViewResolver JavaDoc](http://web.archive.org/web/20190213135405/http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/web/servlet/view/ContentNegotiatingViewResolver.html)
*   [使用 Spring MVC 的 ContentNegotiatingViewResolver 进行内容协商](http://web.archive.org/web/20190213135405/http://www.jroller.com/eyallupu/entry/content_negotiation_using_spring_mvc)
*   [另一个 ContentNegotiatingViewResolver 示例](http://web.archive.org/web/20190213135405/http://blog.anthonychaves.net/2010/02/01/spring-3-0-web-mvc-and-json/)
*   [Spring 3 MVC 和 RSS feed 示例](http://web.archive.org/web/20190213135405/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-rss-feed-example/)
*   [Spring 3 MVC 和 XML 示例](http://web.archive.org/web/20190213135405/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-xml-example/)
*   [Spring 3 MVC 和 JSON 示例](http://web.archive.org/web/20190213135405/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-json-example/)

[spring mvc](http://web.archive.org/web/20190213135405/http://www.mkyong.com/tag/spring-mvc/) [spring3](http://web.archive.org/web/20190213135405/http://www.mkyong.com/tag/spring3/)







