# spring MVC InternalResourceViewResolver 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-internalresourceviewresolver-example/>

在 Spring MVC 中，**InternalResourceViewResolver**用于根据预定义的 URL 模式解析“内部资源视图”(简单来说，就是最终输出，jsp 或 htmp 页面)。此外，它允许您添加一些预定义的前缀或后缀到视图名称(前缀+视图名称+后缀)，并生成最终的视图页面 URL。

**What’s internal resource views?**
In Spring MVC or any web application, for good practice, it’s always recommended to put the entire views or JSP files under “WEB-INF” folder, to protect it from direct access via manual entered URL. Those views under “WEB-INF” folder are named as internal resource views, as it’s only accessible by the servlet or Spring’s controllers class.

以下示例显示了 InternalResourceViewResolver 的工作方式:

## 1.控制器

一个返回视图的控制器类，名为“ **WelcomePage** ”。

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

## 2.InternalResourceViewResolver

在 Spring 的 bean 配置文件中注册**InternalResourceViewResolver**bean。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

   <bean 
   class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

	<!-- Register the bean -->
	<bean class="com.mkyong.common.controller.WelcomeController" />

	<bean id="viewResolver"
    	      class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
              <property name="prefix">
                  <value>/WEB-INF/pages/</value>
               </property>
              <property name="suffix">
                 <value>.jsp</value>
              </property>
        </bean>

</beans> 
```

现在，Spring 将以如下方式解析视图的名称" **WelcomePage** ":

> 前缀+视图名+后缀=/we b-INF/pages/**WelcomPage**。jsp

## 下载源代码

Download it – [SpringMVC-InternalResourceViewResolver-Example.zip](http://web.archive.org/web/20220907153704/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-InternalResourceViewResolver-Example.zip) (7 KB)

## 参考

1.  [InternalResourceViewResolver 文档](http://web.archive.org/web/20220907153704/http://static.springsource.org/spring/docs/2.5.6/api/org/springframework/web/servlet/view/InternalResourceViewResolver.html)

<input type="hidden" id="mkyong-current-postId" value="6538">