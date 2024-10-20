# Spring MVC 参数 MethodNameResolver 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-parametermethodnameresolver-example/>

**parametermethodname resolver**，MultiActionController 方法名解析器通过请求参数名将 URL 映射到方法名**，参数名可通过“ **paramName** 属性定制。请参见以下示例:**

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

 ## 2.ParameterMethodNameResolver

配置了**参数方法名称解析器**，通过“**参数名称**属性定义参数名称:

```java
 <beans ...>

 <bean 
  class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

  <bean class="com.mkyong.common.controller.CustomerController">
     <property name="methodNameResolver">
	<bean class="org.springframework.web.servlet.mvc.multiaction.ParameterMethodNameResolver">
	   <property name="paramName" value="action"/>
	</bean>
     </property>
   </bean>

</beans> 
```

现在，URL 将通过“action”请求参数名称映射到方法名称:

1.  /customer/*。htm **？action = add**–>add()方法
2.  /customer/whatever.htm **？action = add**–>add()方法
3.  /customer/*。htm **？action = update**–>update()方法
4.  /customer/*。htm **？action = delete**–>delete()方法
5.  /customer/*。htm **？action = list**–>list()方法

*附注“ ***** ”指任何文本。*

**Note**
By default, **MultiActionController** is used the [InternalPathMethodNameResolver](http://web.archive.org/web/20190212162210/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/) to map URL to the corresponds method name. ## 下载源代码

Download it – [SpringMVC-ParameterMethodNameResolver-Example.zip](http://web.archive.org/web/20190212162210/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-ParameterMethodNameResolver-Example.zip) (7KB)

## 参考

1.  [参数 MethodNameResolver Javadoc](http://web.archive.org/web/20190212162210/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/mvc/multiaction/ParameterMethodNameResolver.html)
2.  [Spring MVC multi action controller 示例](http://web.archive.org/web/20190212162210/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/)

[spring mvc](http://web.archive.org/web/20190212162210/http://www.mkyong.com/tag/spring-mvc/)







