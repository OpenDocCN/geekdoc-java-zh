# spring MVC properties methodnameresolver 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-propertiesmethodnameresolver-example/>

**properties methodnameresolver**，一个灵活的 **MultiActionController** 方法名解析器，用来**明确定义 URL 和方法名之间的映射**。请参见以下示例:

## 1.多动作控制器

一个 MultiActionController 示例。

```java
 package com.mkyong.common.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

public class CustomerController extends MultiActionController{

	public ModelAndView add(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerPage", "msg","add() method");

	}

	public ModelAndView delete(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerPage", "msg","delete() method");

	}

	public ModelAndView update(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerPage", "msg","update() method");

	}

	public ModelAndView list(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerPage", "msg","list() method");

	}

} 
```

 ## 2.PropertiesMethodNameResolver

使用**propertiesmethodname resolver**，您可以轻松地将任何 URL 名称映射到对应的方法名称:

```java
 <beans ...>

 <bean 
  class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

 <bean class="com.mkyong.common.controller.CustomerController">
   <property name="methodNameResolver">
    <bean class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
      <property name="mappings">
	<props>
	   <prop key="/customer/a.htm">add</prop>
	   <prop key="/customer/b.htm">update</prop>
	   <prop key="/customer/c.htm">delete</prop>
	   <prop key="/customer/d.htm">list</prop>
	   <prop key="/customer/whatever.htm">add</prop>
	</props>
       </property>
     </bean>
    </property>
  </bean>

</beans> 
```

现在，URL 将按照以下模式映射到方法名:

1.  /customer/a . htm –> add()方法
2.  /customer/b . htm –> update()方法
3.  /customer/c . htm –> delete()方法
4.  /customer/d . htm –> list()方法
5.  /customer/whatever . htm –> add()方法

**Note**
By default, **MultiActionController** is used the [InternalPathMethodNameResolver](http://web.archive.org/web/20190214225817/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/) to map URL to the corresponds method name. ## 下载源代码

Download it – [SpringMVC-PropertiesMethodNameResolver-Example.zip](http://web.archive.org/web/20190214225817/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-PropertiesMethodNameResolver-Example.zip) (7KB)

## 参考

1.  [属性 MethodNameResolver Javadoc](http://web.archive.org/web/20190214225817/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/mvc/multiaction/PropertiesMethodNameResolver.html)
2.  [Spring MVC multi action controller 示例](http://web.archive.org/web/20190214225817/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/)

[spring mvc](http://web.archive.org/web/20190214225817/http://www.mkyong.com/tag/spring-mvc/)







