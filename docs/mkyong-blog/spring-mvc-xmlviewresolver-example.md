# Spring MVC XmlViewResolver 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-xmlviewresolver-example/>

在 Spring MVC 中， **XmlViewResolver** 用于根据 XML 文件中的视图 beans 解析“视图名”。默认情况下，`XmlViewResolver`将从 **/WEB-INF/views.xml** 加载视图 beans，但是，该位置可以通过“ **location** 属性覆盖:

```java
 <beans ...>
	<bean class="org.springframework.web.servlet.view.XmlViewResolver">
	   <property name="location">
		<value>/WEB-INF/spring-views.xml</value>
	   </property>
	</bean>
</beans> 
```

在上面的例子中，它从"**/we b-INF/spring-views . XML**"加载视图 beans。请参见 XmlViewResolver 示例:

## 1.控制器

一个控制器类，返回一个视图，名为“ **WelcomePage** ”。

```java
 //...
public class WelcomeController extends AbstractController{

	@Override
	protected ModelAndView handleRequestInternal(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		ModelAndView model = new ModelAndView("WelcomePage");

		return model;
	}
} 
```

 ## 2.XmlViewResolver

在 Spring 的 bean 配置文件中注册 XmlViewResolver，从"**/we b-INF/Spring-views . XML**"加载视图 bean。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

   <bean 
   class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

	<!-- Register the bean -->
	<bean class="com.mkyong.common.controller.WelcomeController" />

	<bean class="org.springframework.web.servlet.view.XmlViewResolver">
	   <property name="location">
	       <value>/WEB-INF/spring-views.xml</value>
	   </property>
	</bean>

</beans> 
```

 ## 3.查看 beans

“**视图 bean** ”只是在 Spring 的 bean 配置文件中声明的一个普通的 Spring bean，其中

1.  “ **id** 是要解析的“视图名称”。
2.  **类**是视图的类型。
3.  “ **url** 属性是视图的 url 位置。

*文件:spring-views.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="WelcomePage"
		class="org.springframework.web.servlet.view.JstlView">
		<property name="url" value="/WEB-INF/pages/WelcomePage.jsp" />
	</bean>

</beans> 
```

**How it works ?**
When a view name “**WelcomPage**” is returned by controller, the `XmlViewResolver` will find the bean id “**WelcomPage**” in “spring-views.xml” file, and return the corresponds view’s URL “**/WEB-INF/pages/WelcomPage.jsp**” back to the `DispatcherServlet`.

## 下载源代码

Download it – [SpringMVC-XmlViewResolver-Example.zip](http://web.archive.org/web/20190212044514/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-XmlViewResolver-Example.zip) (7KB)

## 参考

1.  [XmlViewResolver 文档](http://web.archive.org/web/20190212044514/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/view/XmlViewResolver.html)

[spring mvc](http://web.archive.org/web/20190212044514/http://www.mkyong.com/tag/spring-mvc/) [xml](http://web.archive.org/web/20190212044514/http://www.mkyong.com/tag/xml/)







