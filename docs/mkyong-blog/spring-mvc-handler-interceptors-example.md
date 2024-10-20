# Spring MVC 处理程序拦截器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-handler-interceptors-example/>

Spring MVC 允许您通过处理程序拦截器来拦截 web 请求。处理程序拦截器必须实现 **HandlerInterceptor** 接口，该接口包含三个方法:

1.  **preHandle()**–在处理程序执行前调用，返回一个布尔值，“true”:继续处理程序执行链；“false”，停止执行链并返回。
2.  **post handle()**–在处理程序执行后调用，允许在将 ModelAndView 对象呈现到视图页面之前对其进行操作。
3.  **after completion()**–在完成请求完成后调用。很少使用，找不到任何用例。

在本教程中，您将创建两个处理程序拦截器来展示 **HandlerInterceptor** 的用法。

1.  **execute time interceptor**–拦截 web 请求，记录控制器执行时间。
2.  **维护拦截器**–拦截 web 请求，检查当前时间是否在维护时间之间，如果是，则重定向到维护页面。

**Note**
It’s recommended to extend the **HandlerInterceptorAdapter** for the convenient default implementations.

## 1.ExecuteTimeInterceptor

截取控制器执行前后的时间，记录执行的开始和结束时间，保存到已有的截取控制器的 modelAndView 中，以便以后显示。

*文件:ExecuteTimeInterceptor.java*

```java
 package com.mkyong.common.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.log4j.Logger;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

public class ExecuteTimeInterceptor extends HandlerInterceptorAdapter{

	private static final Logger logger = Logger.getLogger(ExecuteTimeInterceptor.class);

	//before the actual handler will be executed
	public boolean preHandle(HttpServletRequest request, 
		HttpServletResponse response, Object handler)
	    throws Exception {

		long startTime = System.currentTimeMillis();
		request.setAttribute("startTime", startTime);

		return true;
	}

	//after the handler is executed
	public void postHandle(
		HttpServletRequest request, HttpServletResponse response, 
		Object handler, ModelAndView modelAndView)
		throws Exception {

		long startTime = (Long)request.getAttribute("startTime");

		long endTime = System.currentTimeMillis();

		long executeTime = endTime - startTime;

		//modified the exisitng modelAndView
		modelAndView.addObject("executeTime",executeTime);

		//log it
		if(logger.isDebugEnabled()){
		   logger.debug("[" + handler + "] executeTime : " + executeTime + "ms");
		}
	}
} 
```

## 2.维护拦截器

在控制器执行前拦截，检查当前时间是否在维护时间之间，如果是则重定向到维护页面；否则继续执行链。

*文件:MaintenanceInterceptor.java*

```java
 package com.mkyong.common.interceptor;

import java.util.Calendar;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

public class MaintenanceInterceptor extends HandlerInterceptorAdapter{

	private int maintenanceStartTime;
	private int maintenanceEndTime;
	private String maintenanceMapping;

	public void setMaintenanceMapping(String maintenanceMapping) {
		this.maintenanceMapping = maintenanceMapping;
	}

	public void setMaintenanceStartTime(int maintenanceStartTime) {
		this.maintenanceStartTime = maintenanceStartTime;
	}

	public void setMaintenanceEndTime(int maintenanceEndTime) {
		this.maintenanceEndTime = maintenanceEndTime;
	}

	//before the actual handler will be executed
	public boolean preHandle(HttpServletRequest request, 
			HttpServletResponse response, Object handler)
	    throws Exception {

		Calendar cal = Calendar.getInstance();
		int hour = cal.get(cal.HOUR_OF_DAY);

		if (hour >= maintenanceStartTime && hour <= maintenanceEndTime) {
			//maintenance time, send to maintenance page
			response.sendRedirect(maintenanceMapping);
			return false;
		} else {
			return true;
		}

	}
} 
```

## 3.启用处理程序拦截器

要启用它，请将您的处理程序拦截器类放入处理程序映射“**拦截器**”属性中。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
				<prop key="/welcome.htm">welcomeController</prop>
			</props>
		</property>
		<property name="interceptors">
			<list>
				<ref bean="maintenanceInterceptor" />
				<ref bean="executeTimeInterceptor" />
			</list>
		</property>
	</bean>

	<bean
	class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping">
		<property name="interceptors">
			<list>
				<ref bean="executeTimeInterceptor" />
			</list>
		</property>
	</bean>

	<bean id="welcomeController" 
                  class="com.mkyong.common.controller.WelcomeController" />
	<bean class="com.mkyong.common.controller.MaintenanceController" />

	<bean id="executeTimeInterceptor" 
                 class="com.mkyong.common.interceptor.ExecuteTimeInterceptor" />

	<bean id="maintenanceInterceptor" 
                class="com.mkyong.common.interceptor.MaintenanceInterceptor">
		<property name="maintenanceStartTime" value="23" />
		<property name="maintenanceEndTime" value="24" />
		<property name="maintenanceMapping" value="/SpringMVC/maintenance.htm" />
	</bean>

	<bean id="viewResolver"
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix">
			<value>/WEB-INF/pages/</value>
		</property>
		<property name="suffix">
			<value>.jsp</value>
		</property>
	</bean>

</beans> 
```

## 下载源代码

Download it –[SpringMVC-HandlerInterceptor-Example.zip](http://web.archive.org/web/20210506232204/http://www.mkyong.com/wp-content/uploads/2010/07/SpringMVC-HandlerInterceptor-Example.zip) (8 KB)

## 参考

1.  [HandlerInterceptorAdapter 文档](http://web.archive.org/web/20210506232204/http://static.springsource.org/spring/docs/2.5.6/api/org/springframework/web/portlet/handler/HandlerInterceptorAdapter.html)
2.  [处理接收器文档](http://web.archive.org/web/20210506232204/http://static.springsource.org/spring/docs/2.5.6/api/org/springframework/web/portlet/HandlerInterceptor.html)
3.  [ControllerClassNameHandlerMapping 示例](http://web.archive.org/web/20210506232204/http://www.mkyong.com/spring-mvc/spring-mvc-controllerclassnamehandlermapping-example/)
4.  [SimpleUrlHandlerMapping 示例](http://web.archive.org/web/20210506232204/http://www.mkyong.com/spring-mvc/spring-mvc-simpleurlhandlermapping-example/)

Tags : [interceptor](http://web.archive.org/web/20210506232204/https://mkyong.com/tag/interceptor/) [spring mvc](http://web.archive.org/web/20210506232204/https://mkyong.com/tag/spring-mvc/)<input type="hidden" id="mkyong-current-postId" value="6494">