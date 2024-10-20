# Spring MVC 多动作控制器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/>

在 Spring MVC 应用程序中， **MultiActionController** 用于将相关的动作分组到一个控制器中，方法处理程序必须遵循以下签名:

```java
 public (ModelAndView | Map | String | void) actionName(
		HttpServletRequest, HttpServletResponse [,HttpSession] [,CommandObject]); 
```

## 1.多动作控制器

请参见 MultiActionController 示例。

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

配置了**控制器 class name handler 映射**。

```java
 <beans ...>

 <bean 
  class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

  <bean class="com.mkyong.common.controller.CustomerController" />

</beans> 
```

 ## 2.映射示例

现在，重新请求的 URL 将按照以下模式映射到方法名:

1.  **客户**控制人—>**/客户/***
2.  /customer/ **添加**。htm->**添加()**
3.  /customer/ **删除**。htm->**删除()**
4.  /customer/ **更新**。htm->**更新()**
5.  /客户/ **列表**。htm->**列表()**

 ## 3.InternalPathMethodNameResolver

InternalPathMethodNameResolver 是默认的 **MultiActionController** 实现，用于将 URL 映射到方法名。但是，您仍然可以给方法名添加前缀或后缀:

```java
 <beans ...>
 <bean 
  class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

  <bean class="com.mkyong.common.controller.CustomerController">
     <property name="methodNameResolver">
	<bean class="org.springframework.web.servlet.mvc.multiaction.InternalPathMethodNameResolver">
	   <property name="prefix" value="test" />
	   <property name="suffix" value="Customer" />
	</bean>
     </property>
   </bean>
</beans> 
```

现在，URL 将按照以下模式映射到方法名:

1.  **客户**控制人—>**/客户/***
2.  /customer/ **添加**。htm->测试**添加**客户()
3.  /customer/ **删除**。htm->测试**删除**客户()
4.  /customer/ **更新**。htm—>测试**更新**客户()
5.  /客户/ **列表**。htm—>测试**列表 C** 客户()

Note
With annotation, the MultiActionController is more easy to configure, visit this [MultiActionController annotation example](http://web.archive.org/web/20190214224056/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-annotation-example/) for detail.

## 下载源代码

Download it – [SpringMVC-MultiActionController-Example.zip](http://web.archive.org/web/20190214224056/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-MultiActionController-Example.zip) (7KB)

## 参考

1.  [多动作控制器 Javadoc](http://web.archive.org/web/20190214224056/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/mvc/multiaction/MultiActionController.html)
2.  [InternalPathMethodNameResolver Javadoc](http://web.archive.org/web/20190214224056/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/mvc/multiaction/InternalPathMethodNameResolver.html)

[spring mvc](http://web.archive.org/web/20190214224056/http://www.mkyong.com/tag/spring-mvc/)







