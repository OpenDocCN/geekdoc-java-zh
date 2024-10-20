# Spring MVC 单选按钮和单选按钮示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-radiobutton-and-radiobuttons-example/>

在 Spring MVC 中， **<表单:radiobutton / >** 用于渲染一个 HTML 单选按钮，单选按钮值硬编码在 JSP 页面内部；而 **<表单:radiobuttons / >** 用于呈现多个单选按钮，单选按钮值在运行时生成。

在本教程中，我们将向您展示如何使用 **<表单:radiobutton / >** 和 **<表单:radio button/>**。

## 1.<radiobutton></radiobutton>

生成一个单选按钮，并硬编码该值。

```java
 public class Customer{
	String sex;
	//...
} 
```

```java
 <form:radiobutton path="sex" value="M"/>Male 
<form:radiobutton path="sex" value="F"/>Female 
```

**Default value…**
To make the “**Male**” as the default selected value in above radio buttons, just set the “**sex**” property to “**M**“, for example :

```java
 public class Customer{
	String sex = "M";
	//...
} 
```

或者

```java
 //SimpleFormController...
@Override
protected Object formBackingObject(HttpServletRequest request)
	throws Exception {

	Customer cust = new Customer();
	//Make "Male" as the default radio button selected value
	cust.setSex("M");

	return cust;

} 
```

 ## 2.<radiobuttons></radiobuttons>

生成多个单选按钮，这些值在运行时生成。

```java
 //SimpleFormController...
protected Map referenceData(HttpServletRequest request) throws Exception {

	Map referenceData = new HashMap();

	List<String> numberList = new ArrayList<String>();
	numberList.add("Number 1");
	numberList.add("Number 2");
	numberList.add("Number 3");
	numberList.add("Number 4");
	numberList.add("Number 5");
	referenceData.put("numberList", numberList);

	return referenceData;		
} 
```

```java
 <form:radiobuttons path="favNumber" items="${numberList}"  /> 
```

**Default value…**
To make the “**Number 1**” as the default selected value in above radio buttons, just set the “**favNumber**” property to “**Number 1**“, for example :

```java
 public class Customer{
	String favNumber = "Number 1";
	//...
} 
```

或者

```java
 //SimpleFormController...
@Override
protected Object formBackingObject(HttpServletRequest request)
	throws Exception {

	Customer cust = new Customer();
	//Make "Number 1" as the default radio button selected value
	cust.setFavNumber("Number 1")

	return cust;	
} 
```

**Note**
For radio button selection, as long as the “**path**” or “**property**” is equal to the “**radio button value**“, the radio button will be selected automatically. ## 完整的单选按钮示例

让我们来看一个完整的 Spring MVC 单选按钮示例:

## 1.模型

存储单选按钮值的客户模型类。

*文件:Customer.java*

```java
 package com.mkyong.customer.model;

public class Customer{

	String favNumber;
	String sex;

	public String getFavNumber() {
		return favNumber;
	}
	public void setFavNumber(String favNumber) {
		this.favNumber = favNumber;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}

} 
```

## 2.控制器

一个`SimpleFormController`来处理表单单选按钮的值。将单选按钮“M”设为默认选定值。

*文件:RadioButtonController.java*

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

public class RadioButtonController extends SimpleFormController{

	public RadioButtonController(){
		setCommandClass(Customer.class);
		setCommandName("customerForm");
	}

	@Override
	protected Object formBackingObject(HttpServletRequest request)
		throws Exception {

		Customer cust = new Customer();
		//Make "Make" as default radio button checked value
		cust.setSex("M");

		return cust;
	}

	@Override
	protected ModelAndView onSubmit(HttpServletRequest request,
		HttpServletResponse response, Object command, BindException errors)
		throws Exception {

		Customer customer = (Customer)command;
		return new ModelAndView("CustomerSuccess","customer",customer);
	}

	protected Map referenceData(HttpServletRequest request) throws Exception {

		Map referenceData = new HashMap();

		List<String> numberList = new ArrayList<String>();
		numberList.add("Number 1");
		numberList.add("Number 2");
		numberList.add("Number 3");
		numberList.add("Number 4");
		numberList.add("Number 5");
		referenceData.put("numberList", numberList);

		return referenceData;
	}
} 
```

## 3.验证器

一个简单的表单验证器，确保“**性别**”和“**号码**”单选按钮被选中。

*文件:RadioButtonValidator.java*

```java
 package com.mkyong.customer.validator;

import org.springframework.validation.Errors;
import org.springframework.validation.ValidationUtils;
import org.springframework.validation.Validator;

import com.mkyong.customer.model.Customer;

public class RadioButtonValidator implements Validator{

	@Override
	public boolean supports(Class clazz) {
		//just validate the Customer instances
		return Customer.class.isAssignableFrom(clazz);
	}

	@Override
	public void validate(Object target, Errors errors) {

		Customer cust = (Customer)target;

		ValidationUtils.rejectIfEmptyOrWhitespace(errors, "sex", "required.sex");
		ValidationUtils.rejectIfEmptyOrWhitespace(errors, "favNumber", "required.favNumber");

	}
} 
```

*文件:message.properties*

```java
 required.sex = Please select a sex!
required.favNumber = Please select a number! 
```

## 4.视角

一个 JSP 页面展示了 Spring 的表单标签**<form:radio button/>**和**<form:radio button/>**的使用。

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
	<h2>Spring's form radio button example</h2>

	<form:form method="POST" commandName="customerForm">
		<form:errors path="*" cssClass="errorblock" element="div" />
		<table>
			<tr>
				<td>Sex :</td>
				<td><form:radiobutton path="sex" value="M" />Male <form:radiobutton
					path="sex" value="F" />Female</td>
				<td><form:errors path="sex" cssClass="error" /></td>
			</tr>
			<tr>
				<td>Choose a number :</td>
				<td><form:radiobuttons path="favNumber" items="${numberList}" />
                                </td>
				<td><form:errors path="favNumber" cssClass="error" /></td>
			</tr>
			<tr>
				<td colspan="3"><input type="submit" /></td>
			</tr>
		</table>
	</form:form>

</body>
</html> 
```

使用 JSTL 循环遍历提交的单选按钮值，并显示它。

*文件:CustomerSuccess.jsp*

```java
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<html>
<body>
	<h2>Spring's form radio button example</h2>
	Sex : ${customer.sex}
	<br /> Favourite Number : ${customer.favNumber}
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

	<bean class="com.mkyong.customer.controller.RadioButtonController">
		<property name="formView" value="CustomerForm" />
		<property name="successView" value="CustomerSuccess" />

		<!-- Map a validator -->
		<property name="validator">
			<bean class="com.mkyong.customer.validator.RadioButtonValidator" />
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

访问页面-**http://localhost:8080/SpringMVCForm/radio button . htm**

![SpringMVC-RadioButton-Example-1](img/7679850c0f61d8fe304ce622d5fa47df.png "SpringMVC-RadioButton-Example-1")

如果用户在提交表单时没有选择任何单选按钮值，则显示并突出显示错误消息。

![SpringMVC-RadioButton-Example-2](img/f25ae3411c7c6094492e1e4374bd706c.png "SpringMVC-RadioButton-Example-2")

如果表单提交成功，只需显示已提交的单选按钮值。

![SpringMVC-RadioButton-Example-3](img/c6c53e7ea10cb6d1a87e1af2a2550fab.png "SpringMVC-RadioButton-Example-3")

## 下载源代码

Download it – [SpringMVCForm-RadioButton-Example.zip](http://web.archive.org/web/20190224144757/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVCForm-RadioButton-Example.zip) (9KB)[radio button](http://web.archive.org/web/20190224144757/http://www.mkyong.com/tag/radio-button/) [spring mvc](http://web.archive.org/web/20190224144757/http://www.mkyong.com/tag/spring-mvc/)







