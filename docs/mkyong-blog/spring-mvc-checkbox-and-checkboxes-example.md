# Spring MVC 复选框和复选框示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-checkbox-and-checkboxes-example/>

在 Spring MVC 中， **<表单:checkbox / >** 用于呈现一个 HTML 的复选框字段，复选框值在 JSP 页面内部是硬编码的；当 **<表单:checkboxes / >** 用于呈现多个复选框时，复选框值是在运行时生成的。

在本教程中，我们向您展示了 3 种呈现 HTML 复选框字段的不同方式:

## 1.<checkbox>–单个复选框</checkbox>

用一个布尔值生成一个经典的单个复选框。

```java
 public class Customer{
	boolean receiveNewsletter;
	//...
} 
```

```java
 <form:checkbox path="receiveNewsletter" /> 
```

**Checked by default…**
If you set the “**receiveNewsletter**” boolean value to true, this checkbox will be checked. For example :

```java
 public class Customer{
	boolean receiveNewsletter = true;
	//...
} 
```

 ## 2.<checkbox>–多个复选框</checkbox>

生成多个复选框并硬编码值。

```java
 public class Customer{
	String [] favLanguages;
	//...
} 
```

```java
 <form:checkbox path="favLanguages" value="Java"/>Java 
<form:checkbox path="favLanguages" value="C++"/>C++ 
<form:checkbox path="favLanguages" value=".Net"/>.Net 
```

**Checked by default…**
If you want to make the checkbox with value “Java” is checked by default, you can initialize the “**favLanguages**” property with value “Java”. For example :

```java
 //SimpleFormController...
        @Override
	protected Object formBackingObject(HttpServletRequest request)
		throws Exception {

		Customer cust = new Customer();
		cust.setFavLanguages(new String []{"Java"});

		return cust;

	} 
```

 ## 3.<checkboxes>–多个复选框</checkboxes>

为复选框值生成一个运行时列表，并将其链接到 Spring 的表单标签 **<表单:复选框>** 。

```java
 //SimpleFormController...
	protected Map referenceData(HttpServletRequest request) throws Exception {

		Map referenceData = new HashMap();
		List<String> webFrameworkList = new ArrayList<String>();
		webFrameworkList.add("Spring MVC");
		webFrameworkList.add("Struts 1");
		webFrameworkList.add("Struts 2");
		webFrameworkList.add("Apache Wicket");
		referenceData.put("webFrameworkList", webFrameworkList);

		return referenceData;
	} 
```

```java
 <form:checkboxes items="${webFrameworkList}" path="favFramework" /> 
```

**Checked by default…**
If you want to make 2 checkboxes with value “Spring MVC” and “Struts 2” are checked by default, you can initialize the “**favFramework**” property with value “Spring MVC” and “Struts 2”. Fro example :

```java
 //SimpleFormController...
        @Override
	protected Object formBackingObject(HttpServletRequest request)
		throws Exception {

		Customer cust = new Customer();
		cust.setFavFramework(new String []{"Spring MVC","Struts 2"});

		return cust;
	} 
```

**Note**

```java
 <form:checkboxes items="${dynamic-list}" path="property-to-store" /> 
```

对于多个复选框，只要“**路径**或“**属性**值等于任意一个“**复选框值—$ { dynamic-list }**，匹配的复选框将被自动勾选。

## 完整复选框示例

让我们来看一个完整的 Spring MVC 复选框示例:

## 1.模型

存储复选框值的客户模型类。

*文件:Customer.java*

```java
 package com.mkyong.customer.model;

public class Customer{

	//checkbox
	boolean receiveNewsletter = true; //checked it
	String [] favLanguages;
	String [] favFramework;

	public String[] getFavFramework() {
		return favFramework;
	}
	public void setFavFramework(String[] favFramework) {
		this.favFramework = favFramework;
	}
	public boolean isReceiveNewsletter() {
		return receiveNewsletter;
	}
	public void setReceiveNewsletter(boolean receiveNewsletter) {
		this.receiveNewsletter = receiveNewsletter;
	}
	public String[] getFavLanguages() {
		return favLanguages;
	}
	public void setFavLanguages(String[] favLanguages) {
		this.favLanguages = favLanguages;
	}
} 
```

## 2.控制器

处理表单复选框值的 SimpleFormController。

*文件:CheckBoxController.java*

```java
 package com.mkyong.customer.controller;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.validation.BindException;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.SimpleFormController;
import com.mkyong.customer.model.Customer;

public class CheckBoxController extends SimpleFormController{

	public CheckBoxController(){
		setCommandClass(Customer.class);
		setCommandName("customerForm");
	}

	@Override
	protected Object formBackingObject(HttpServletRequest request)
		throws Exception {

		Customer cust = new Customer();

		//Make "Spring MVC" and "Struts 2" as default checked value
		cust.setFavFramework(new String []{"Spring MVC","Struts 2"});

		//Make "Java" as default checked value
		cust.setFavLanguages(new String []{"Java"});

		return cust;

	}

	@Override
	protected ModelAndView onSubmit(HttpServletRequest request,
		HttpServletResponse response, Object command, BindException errors)
		throws Exception {

		Customer customer = (Customer)command;
		return new ModelAndView("CustomerSuccess","customer",customer);

	}

	//Generate the data for web framework multiple checkboxes
	protected Map referenceData(HttpServletRequest request) throws Exception {

		Map referenceData = new HashMap();
		List<String> webFrameworkList = new ArrayList<String>();
		webFrameworkList.add("Spring MVC");
		webFrameworkList.add("Struts 1");
		webFrameworkList.add("Struts 2");
		webFrameworkList.add("Apache Wicket");
		referenceData.put("webFrameworkList", webFrameworkList);

		return referenceData;

	}
} 
```

## 3.验证器

一个简单的表单验证器确保“ **favLanguages** ”属性不为空。

*文件:CheckBoxValidator.java*

```java
 package com.mkyong.customer.validator;

import org.springframework.validation.Errors;
import org.springframework.validation.Validator;
import com.mkyong.customer.model.Customer;

public class CheckBoxValidator implements Validator{

	@Override
	public boolean supports(Class clazz) {
		//just validate the Customer instances
		return Customer.class.isAssignableFrom(clazz);
	}

	@Override
	public void validate(Object target, Errors errors) {

		Customer cust = (Customer)target;

		if(cust.getFavLanguages().length==0){
			errors.rejectValue("favLanguages", "required.favLanguages");
		}
	}
} 
```

*文件:message.properties*

```java
 required.favLanguages = Please select at least a favorite programming language! 
```

## 4.视角

一个 JSP 页面展示了 Spring 的表单标签 **<表单的使用:checkbox / >** 和 **<表单:checkboxes / >** 。

*文件:CustomerForm.jsp*

```java
 <%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>
<html>
<head>
<style>
.error {
	color: #ff0000;
}

.errorblock {
	color: #000;
	background-color: #ffEEEE;
	border: 3px solid #ff0000;
	padding: 8px;
	margin: 16px;
}
</style>
</head>

<body>
	<h2>Spring's form checkbox example</h2>

	<form:form method="POST" commandName="customerForm">
		<form:errors path="*" cssClass="errorblock" element="div" />
		<table>
			<tr>
				<td>Subscribe to newsletter? :</td>
				<td><form:checkbox path="receiveNewsletter" /></td>
				<td><form:errors path="receiveNewsletter" cssClass="error" /></td>
			</tr>
			<tr>
				<td>Favourite Languages :</td>
				<td>
                                       <form:checkbox path="favLanguages" value="Java" />Java 
                                       <form:checkbox path="favLanguages" value="C++" />C++ 
                                       <form:checkbox path="favLanguages" value=".Net" />.Net
                                </td>
				<td><form:errors path="favLanguages" cssClass="error" />
				</td>
			</tr>
			<tr>
				<td>Favourite Web Frameworks :</td>
				<td><form:checkboxes items="${webFrameworkList}"
						path="favFramework" /></td>
				<td><form:errors path="favFramework" cssClass="error" /></td>
			</tr>
			<tr>
				<td colspan="3"><input type="submit" /></td>
			</tr>
		</table>
	</form:form>

</body>
</html> 
```

使用 JSTL 循环显示已提交的复选框值。

*文件:CustomerSuccess.jsp*

```java
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<html>
<body>
	<h2>Spring's form checkbox example</h2>

	Receive Newsletter : ${customer.receiveNewsletter}
	<br />

         Favourite Languages :
	<c:forEach items="${customer.favLanguages}" var="current">
		[<c:out value="${current}" />]
	</c:forEach>
	<br />

         Favourite Web Frameworks :
	<c:forEach items="${customer.favFramework}" var="current">
		[<c:out value="${current}" />]
	</c:forEach>
	<br />
</body>
</html> 
```

## 5.弹簧豆配置

全部链接起来~

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

  <bean
  class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

	<bean class="com.mkyong.customer.controller.CheckBoxController">
		<property name="formView" value="CustomerForm" />
		<property name="successView" value="CustomerSuccess" />

		<!-- Map a validator -->
		<property name="validator">
			<bean class="com.mkyong.customer.validator.CheckBoxValidator" />
		</property>
	</bean>

	<!-- Register the Customer.properties -->
	<bean id="messageSource"
		class="org.springframework.context.support.ResourceBundleMessageSource">
		<property name="basename" value="message" />
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

## 6.演示

访问页面-**http://localhost:8080/SpringMVCForm/checkbox . htm**

![SpringMVC-CheckBox-Example-1](img/09ba8666b9a2063f85486cea12b73ad7.png "SpringMVC-CheckBox-Example-1")

如果用户在提交表单时没有选择任何语言复选框值，则显示并突出显示错误消息。

![SpringMVC-CheckBox-Example-2](img/35424402f39f1eea8c69e1a0388680ac.png "SpringMVC-CheckBox-Example-2")

如果表单提交成功，只需显示已提交的复选框值。

![SpringMVC-CheckBox-Example-3](img/ab7a05baeff90b2565f269f7f03bae0c.png "SpringMVC-CheckBox-Example-3")

## 下载源代码

Download it – [SpringMVCForm-CheckBox-Example.zip](http://web.archive.org/web/20190309053750/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVCForm-CheckBox-Example.zip) (10KB)

## 参考

1.  [http://forum.springsource.org/showthread.php?t=80088&高亮显示=复选框](http://web.archive.org/web/20190309053750/http://forum.springsource.org/showthread.php?t=80088&highlight=checkboxes)
2.  [http://forum.springsource.org/showthread.php?t=79133&高亮显示=复选框](http://web.archive.org/web/20190309053750/http://forum.springsource.org/showthread.php?t=79133&highlight=checkboxes)
3.  [http://forum.springsource.org/showthread.php?t=46950&高亮=复选框](http://web.archive.org/web/20190309053750/http://forum.springsource.org/showthread.php?t=46950&highlight=checkbox)
4.  [http://tomasjurman . blogspot . com/2009/12/tag-checkboxes-in-spring-MVC . html](http://web.archive.org/web/20190309053750/http://tomasjurman.blogspot.com/2009/12/tag-checkboxes-in-spring-mvc.html)
5.  [http://www . mkyong . com/struts 2/how-to-set-default-value-for-multiple-checkboxes-in-struts-2/](http://web.archive.org/web/20190309053750/http://www.mkyong.com/struts2/how-to-set-default-value-for-multiple-checkboxes-in-struts-2/)

[checkbox](http://web.archive.org/web/20190309053750/http://www.mkyong.com/tag/checkbox/) [spring mvc](http://web.archive.org/web/20190309053750/http://www.mkyong.com/tag/spring-mvc/)







