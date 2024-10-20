# Spring MVC 国际化示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-internationalization-example/>

在 Spring MVC 应用程序中，附带了几个“**locale solver**”来支持国际化或多语言特性。在本教程中，它显示一个简单的欢迎页面，显示来自属性文件的消息，并根据所选的语言链接更改区域设置。

## 1.项目文件夹

这个例子的目录结构。

![SpringMVC-Internationalization-Folder](img/0847a298883d9dec4a40742fa6a0ba1e.png "SpringMVC-Internationalization-Folder")

## 2.属性文件

两个属性文件存储英文和中文信息。

**welcome.properties**

```java
 welcome.springmvc = Happy learning Spring MVC 
```

**welcome_zh_CN.properties**

```java
 welcome.springmvc = \u5feb\u4e50\u5b66\u4e60 Spring MVC 
```

**Note**
For UTF-8 or non-English characters , you can encode it with [native2ascii](http://web.archive.org/web/20221121171937/http://www.mkyong.com/java/java-convert-chinese-character-to-unicode-with-native2ascii/) tool.

## 3.控制器

控制器类，这里没什么特别的，所有的区域设置都是在 Spring 的 bean 配置文件中配置的。

```java
 package com.mkyong.common.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.AbstractController;

public class WelcomeController extends AbstractController{

	@Override
	protected ModelAndView handleRequestInternal(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		ModelAndView model = new ModelAndView("WelcomePage");
		return model;
	}

} 
```

## 4.弹簧配置

为了使 Spring MVC 应用程序支持国际化，注册两个 beans:

**1。SessionLocaleResolver**
注册一个“SessionLocaleResolver”bean，将其命名为完全相同的字符“ **localeResolver** ”。它通过从用户的会话中获取预定义的属性来解析区域设置。

**Note**
If you do not register any “localeResolver”, the default **AcceptHeaderLocaleResolver** will be used, which resolves the locale by checking the accept-language header in the HTTP request.

**2。LocaleChangeInterceptor**
注册一个“LocaleChangeInterceptor”拦截器，并将其引用到任何需要支持多语言的处理程序映射。“ **paramName** 是用于设置区域设置的参数值。

在这种情况下，

1.  welcome.htm？language = en–从英语属性文件中获取消息。
2.  welcome.htm？language = zh _ CN–从中文属性文件获取消息。

```java
 <bean id="localeChangeInterceptor"
		class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor">
		<property name="paramName" value="language" />
	</bean>

	<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" >
		<property name="interceptors">
		   <list>
			<ref bean="localeChangeInterceptor" />
		    </list>
		</property>
	</bean> 
```

完整示例见下面
**MVC-dispatcher-servlet . XML**

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="localeResolver"
		class="org.springframework.web.servlet.i18n.SessionLocaleResolver">
		<property name="defaultLocale" value="en" />
	</bean>

	<bean id="localeChangeInterceptor"
		class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor">
		<property name="paramName" value="language" />
	</bean>

	<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" >
		<property name="interceptors">
		   <list>
			<ref bean="localeChangeInterceptor" />
		   </list>
		</property>
	</bean>

	<!-- Register the bean -->
	<bean class="com.mkyong.common.controller.WelcomeController" />

	<!-- Register the welcome.properties -->
	<bean id="messageSource"
		class="org.springframework.context.support.ResourceBundleMessageSource">
		<property name="basename" value="welcome" />
	</bean>

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

## 5.JSP

一个 JSP 页面，包含两个手动更改区域设置的超链接，并使用 **spring:message** 通过检查当前用户的区域设置来显示来自相应属性文件的消息。

**WelcomePage.jsp**

```java
 <%@ page contentType="text/html;charset=UTF-8" %>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %>
<html>
<body>
<h1>Spring MVC internationalization example</h1>

Language : <a href="?language=en">English</a>|<a href="?language=zh_CN">Chinese</a>

<h2>
welcome.springmvc : <spring:message code="welcome.springmvc" text="default text" />
</h2>

Current Locale : ${pageContext.response.locale}

</body>
</html> 
```

**Note**
The ${pageContext.response.locale} can be used to display the current user’s locale.**Warning**
Remember put the “<%@ page contentType=”text/html;charset=UTF-8″ %>” on top of the page, else the page may not able to display the UTF-8 (Chinese) characters properly.

## 7.演示

通过*http://localhost:8080/spring MVC/welcome . htm*访问它，通过点击语言的链接更改区域设置。

**1。英语区域设置**–http://localhost:8080/spring MVC/welcome . htm？语言=en

![SpringMVC-Internationalization-Example-1](img/ced7d6325c947a5aae66778cbaf1e202.png "SpringMVC-Internationalization-Example-1")

**2。中文区域设置**–http://localhost:8080/spring MVC/welcome . htm？语言=zh_CN

![SpringMVC-Internationalization-Example-2](img/9f01f4a5538a816332273453b9e6d00e.png "SpringMVC-Internationalization-Example-2")

## 下载源代码

Download it – [SpringMVC-Internationalization-Example.zip](http://web.archive.org/web/20221121171937/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-Internationalization-Example.zip) (8KB)

## 参考

1.  [Spring MVC locale solver 文档](http://web.archive.org/web/20221121171937/http://static.springsource.org/spring/docs/2.5.6/reference/mvc.html#mvc-localeresolver)

<input type="hidden" id="mkyong-current-postId" value="6523">