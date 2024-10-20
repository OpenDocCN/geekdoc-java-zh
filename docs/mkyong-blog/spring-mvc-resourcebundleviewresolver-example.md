# spring MVC ResourceBundleViewResolver 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-resourcebundleviewresolver-example/>

在 Spring MVC 中，**ResourceBundleViewResolver**用于解析“基于视图 beans 中名为”的视图。属性”文件。

默认情况下，`ResourceBundleViewResolver`将从文件 **views.properties** 中加载视图 beans，该文件位于项目类路径的根目录下。但是，这个位置可以通过“ **basename** 属性覆盖，例如，

```java
 <beans ...>
	<bean class="org.springframework.web.servlet.view.ResourceBundleViewResolver">
		<property name="basename" value="spring-views" />
	</bean>
</beans> 
```

在上面的例子中，它从" **spring-views.properties** "加载视图 beans，它位于项目类路径的根。

**Note**
The `ResourceBundleViewResolver` has the ability to load view beans from different resource bundles for different locales, but this use case is rarely required.

ResourceBundleViewResolver 示例向您展示了它的工作原理:

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

 ## 2.ResourceBundleViewResolver

在 Spring 的 bean 配置文件中注册`ResourceBundleViewResolver`，将默认的视图 bean 位置更改为“ **spring-views.properties** ”。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

  <bean 
  class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

	<!-- Register the bean -->
	<bean class="com.mkyong.common.controller.WelcomeController" />

	<bean class="org.springframework.web.servlet.view.ResourceBundleViewResolver">
		<property name="basename" value="spring-views" />
	</bean>

</beans> 
```

 ## 3.查看 beans

将每个视图 bean 声明为普通的资源包样式(键和消息)，其中

1.  " **WelcomePage** "是要匹配的视图名称。
2.  **。**(类)是视图的类型。
3.  **。url** 是视图的 url 位置。

*文件:spring-views.properties*

```java
 WelcomePage.(class)=org.springframework.web.servlet.view.JstlView
WelcomePage.url=/WEB-INF/pages/WelcomePage.jsp 
```

**Note**
Put this “`spring-views.properties`” file on your project class path.**How it works ?**
When view name “**WelcomPage**” is returned by controller, the ResourceBundleViewResolver will find the key start with “**WelcomPage**” in “**spring-views.properties**” file, and return the corresponds view’s URL “**/WEB-INF/pages/WelcomPage.jsp**” back to the DispatcherServlet.

## 下载源代码

Download it – [SpringMVC-ResourceBundleViewResolver-Example.zip](http://web.archive.org/web/20190210095000/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-ResourceBundleViewResolver-Example.zip) (7 KB)

## 参考

1.  [ResourceBundleViewResolver 文档](http://web.archive.org/web/20190210095000/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/view/ResourceBundleViewResolver.html)

[spring mvc](http://web.archive.org/web/20190210095000/http://www.mkyong.com/tag/spring-mvc/)







