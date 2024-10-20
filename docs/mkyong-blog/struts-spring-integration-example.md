# Struts + Spring 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-spring-integration-example/>

下面的教程展示了如何在用 Apache Struts 1.x 开发的 web 应用程序中访问 Spring Ioc 容器中声明的 beans。

Download this Struts 1.x + Spring example – [Struts-Spring-Hibernate-Example.zip](http://web.archive.org/web/20190214233822/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-Spring-Hibernate-Example.zip)

Spring 为 Spring Ioc 容器中声明的访问 beans 提供了“特定于 Struts”的解决方案。

1.  在 Struts 配置文件中注册一个 Spring 的现成 Struts 插件。
2.  更改您的 Struts action 类来扩展 Spring 的 **ActionSupport** 类，它是 Struts Action 类的子类。
3.  **ActionSupport** 为您访问 Spring Ioc 容器中声明的 beans 提供了一个方便的**getWebApplicationContext()**方法。

## 1.Struts + Spring 依赖项

为了与 Struts 1.x 集成，Spring 需要" **spring-web.jar** "和" **spring-struts.jar** "库。你可以从 Spring 网站或者 Maven 下载。
POM . XML

```java
 <!-- Spring framework --> 
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring</artifactId>
		<version>2.5.6</version>
	</dependency>

        <dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-web</artifactId>
		<version>2.5.6</version>
	</dependency>

	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-struts</artifactId>
		<version>2.0.8</version>
	</dependency> 
```

 ## 2.注册 Struts 插件

在您的 struts 配置文件(struts-config.xml)中，注册 Spring 的现成 Struts 插件—“**ContextLoaderPlugIn**”。

**struts-config.xml**

```java
 <struts-config>
    <!-- Spring Struts plugin -->
 	<plug-in className="org.springframework.web.struts.ContextLoaderPlugIn">
		<set-property property="contextConfigLocation"
			value="/WEB-INF/classes/SpringBeans.xml" />
  	</plug-in>
</struts-config> 
```

“ **ContextLoaderPlugIn** ”将处理 Struts 和 Spring 之间的所有集成工作。您可以将 Spring 的 bean xml 文件加载到“ **contextConfigLocation** 属性中。

**SpringBeans.xml**

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<!-- Beans Declaration -->
	<import resource="com/mkyong/customer/spring/CustomerBean.xml"/>

</beans> 
```

 ## 3.春天的行动支持

在 Struts Action 类中，扩展了 Spring " **ActionSupport** "类，通过"**getWebApplicationContext()**"方法获取 Spring 的 bean。

**CustomerBean.xml**

```java
 <bean id="customerBo" class="com.mkyong.customer.bo.impl.CustomerBoImpl" >
   		<property name="customerDao" ref="customerDao" />
   	</bean> 
```

**Struts 动作**

```java
 package com.mkyong.customer.action;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.springframework.web.struts.ActionSupport;

import com.mkyong.customer.bo.CustomerBo;
import com.mkyong.customer.model.Customer;

public class ListCustomerAction extends ActionSupport{

  public ActionForward execute(ActionMapping mapping,ActionForm form,
	HttpServletRequest request,HttpServletResponse response) 
        throws Exception {

	CustomerBo customerBo =
		(CustomerBo) getWebApplicationContext().getBean("customerBo");

	...
	return mapping.findForward("success");

  }
} 
```

完成了。

[integration](http://web.archive.org/web/20190214233822/http://www.mkyong.com/tag/integration/) [spring](http://web.archive.org/web/20190214233822/http://www.mkyong.com/tag/spring/) [struts](http://web.archive.org/web/20190214233822/http://www.mkyong.com/tag/struts/)







