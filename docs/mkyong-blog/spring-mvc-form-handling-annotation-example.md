# Spring MVC 表单处理注释示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-annotation-example/>

在本教程中，我们将向您展示如何在 Spring MVC web 应用程序中使用注释进行表单处理。

**Note**
This annotation-based example is converted from the last Spring MVC [form handling XML-based example](http://web.archive.org/web/20200614235038/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-example/). So, please compare and spots the different.

## 1.简单表单控制器与@控制器

在基于 XML 的 Spring MVC web 应用程序中，您通过扩展`SimpleFormController`类来创建表单控制器。

在基于注释的情况下，可以使用*@控制器*来代替。

*简单表单控制器*

```java
 public class CustomerController extends SimpleFormController{
      //...
} 
```

*标注*

```java
 @Controller
@RequestMapping("/customer.htm")
public class CustomerController{
      //...
} 
```

## 2.formBackingObject()vs request method。得到

在 SimpleFormController 中，可以在 **formBackingObject()** 方法中初始化命令对象进行绑定。在基于注释的方法中，您可以通过用**@ request mapping(method = request method)注释方法名来做同样的事情。**搞定)。

*简单表单控制器*

```java
 @Override
	protected Object formBackingObject(HttpServletRequest request)
		throws Exception {

		Customer cust = new Customer();
		//Make "Spring MVC" as default checked value
		cust.setFavFramework(new String []{"Spring MVC"});

		return cust;
	} 
```

*标注*

```java
 @RequestMapping(method = RequestMethod.GET)
	public String initForm(ModelMap model){

		Customer cust = new Customer();
		//Make "Spring MVC" as default checked value
		cust.setFavFramework(new String []{"Spring MVC"});

		//command object
		model.addAttribute("customer", cust);

		//return form view
		return "CustomerForm";
	} 
```

## 3\. onSubmit()诉 RequestMethod.POST

在 SimpleFormController 中，表单提交由 **onSubmit()** 方法处理。在基于注释的方法中，您可以通过用**@ request mapping(method = request method)注释方法名来做同样的事情。后)**。

*简单表单控制器*

```java
 @Override
	protected ModelAndView onSubmit(HttpServletRequest request,
		HttpServletResponse response, Object command, BindException errors)
		throws Exception {

		Customer customer = (Customer)command;
		return new ModelAndView("CustomerSuccess");

	} 
```

*标注*

```java
 @RequestMapping(method = RequestMethod.POST)
	public String processSubmit(
		@ModelAttribute("customer") Customer customer,
		BindingResult result, SessionStatus status) {

		//clear the command object from the session
		status.setComplete(); 

		//return form success view
		return "CustomerSuccess";

	} 
```

## 4.reference data()vs @ model attribute

在 SimpleFormController 中，通常通过 **referenceData()** 方法将引用数据放入模型中，以便表单视图可以访问它。在基于注释的方法中，您可以通过用 **@ModelAttribute** 注释方法名来做同样的事情。

*简单表单控制器*

```java
 @Override
	protected Map referenceData(HttpServletRequest request) throws Exception {

		Map referenceData = new HashMap();

		//Data referencing for web framework checkboxes
		List<String> webFrameworkList = new ArrayList<String>();
		webFrameworkList.add("Spring MVC");
		webFrameworkList.add("Struts 1");
		webFrameworkList.add("Struts 2");
		webFrameworkList.add("JSF");
		webFrameworkList.add("Apache Wicket");
		referenceData.put("webFrameworkList", webFrameworkList);

		return referenceData;
	} 
```

*弹簧的形状*

```java
 <form:checkboxes items="${webFrameworkList}" path="favFramework" /> 
```

**标注**

```java
 @ModelAttribute("webFrameworkList")
	public List<String> populateWebFrameworkList() {

		//Data referencing for web framework checkboxes
		List<String> webFrameworkList = new ArrayList<String>();
		webFrameworkList.add("Spring MVC");
		webFrameworkList.add("Struts 1");
		webFrameworkList.add("Struts 2");
		webFrameworkList.add("JSF");
		webFrameworkList.add("Apache Wicket");

		return webFrameworkList;
	} 
```

*弹簧的形状*

```java
 <form:checkboxes items="${webFrameworkList}" path="favFramework" /> 
```

## 5.initBinder()与@InitBinder

在 SimpleFormController 中，通过 **initBinder()** 方法定义绑定或注册自定义属性编辑器。在基于注释的方法中，您可以通过用 **@InitBinder** 注释方法名来做同样的事情。

*简单表单控制器*

```java
 protected void initBinder(HttpServletRequest request,
		ServletRequestDataBinder binder) throws Exception {

		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));
    } 
```

*标注*

```java
 @InitBinder
	public void initBinder(WebDataBinder binder) {
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");

		binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));
	} 
```

## 来自验证

在 SimpleFormController 中，您必须通过 XML bean 配置文件注册验证器类并将其映射到控制器类，验证检查和工作流将自动执行。

在基于注释的情况下，您必须显式执行验证器，并手动在 **@Controller** 类中定义验证流。看到不同的:

*简单表单控制器*

```java
 <bean class="com.mkyong.customer.controller.CustomerController">
                <property name="formView" value="CustomerForm" />
		<property name="successView" value="CustomerSuccess" />

		<!-- Map a validator -->
		<property name="validator">
			<bean class="com.mkyong.customer.validator.CustomerValidator" />
		</property>
	</bean> 
```

*标注*

```java
 @Controller
@RequestMapping("/customer.htm")
public class CustomerController{

	CustomerValidator customerValidator;

	@Autowired
	public CustomerController(CustomerValidator customerValidator){
		this.customerValidator = customerValidator;
	}

	@RequestMapping(method = RequestMethod.POST)
	public String processSubmit(
		@ModelAttribute("customer") Customer customer,
		BindingResult result, SessionStatus status) {

		customerValidator.validate(customer, result);

		if (result.hasErrors()) {
			//if validator failed
			return "CustomerForm";
		} else {
			status.setComplete();
			//form success
			return "CustomerSuccess";
		}
	}
	//... 
```

## 完整示例

查看完整的@Controller 示例。

```java
 package com.mkyong.customer.controller;

import java.sql.Date;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.propertyeditors.CustomDateEditor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.support.SessionStatus;

import com.mkyong.customer.model.Customer;
import com.mkyong.customer.validator.CustomerValidator;

@Controller
@RequestMapping("/customer.htm")
public class CustomerController{

	CustomerValidator customerValidator;

	@Autowired
	public CustomerController(CustomerValidator customerValidator){
		this.customerValidator = customerValidator;
	}

	@RequestMapping(method = RequestMethod.POST)
	public String processSubmit(
		@ModelAttribute("customer") Customer customer,
		BindingResult result, SessionStatus status) {

		customerValidator.validate(customer, result);

		if (result.hasErrors()) {
			//if validator failed
			return "CustomerForm";
		} else {
			status.setComplete();
			//form success
			return "CustomerSuccess";
		}
	}

	@RequestMapping(method = RequestMethod.GET)
	public String initForm(ModelMap model){

		Customer cust = new Customer();
		//Make "Spring MVC" as default checked value
		cust.setFavFramework(new String []{"Spring MVC"});

		//Make "Make" as default radio button selected value
		cust.setSex("M");

		//make "Hibernate" as the default java skills selection
		cust.setJavaSkills("Hibernate");

		//initilize a hidden value
		cust.setSecretValue("I'm hidden value");

		//command object
		model.addAttribute("customer", cust);

		//return form view
		return "CustomerForm";
	}

	@ModelAttribute("webFrameworkList")
	public List<String> populateWebFrameworkList() {

		//Data referencing for web framework checkboxes
		List<String> webFrameworkList = new ArrayList<String>();
		webFrameworkList.add("Spring MVC");
		webFrameworkList.add("Struts 1");
		webFrameworkList.add("Struts 2");
		webFrameworkList.add("JSF");
		webFrameworkList.add("Apache Wicket");

		return webFrameworkList;
	}

	@InitBinder
	public void initBinder(WebDataBinder binder) {
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");

		binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));

	}

	@ModelAttribute("numberList")
	public List<String> populateNumberList() {

		//Data referencing for number radiobuttons
		List<String> numberList = new ArrayList<String>();
		numberList.add("Number 1");
		numberList.add("Number 2");
		numberList.add("Number 3");
		numberList.add("Number 4");
		numberList.add("Number 5");

		return numberList;
	}

	@ModelAttribute("javaSkillsList")
	public Map<String,String> populateJavaSkillList() {

		//Data referencing for java skills list box
		Map<String,String> javaSkill = new LinkedHashMap<String,String>();
		javaSkill.put("Hibernate", "Hibernate");
		javaSkill.put("Spring", "Spring");
		javaSkill.put("Apache Wicket", "Apache Wicket");
		javaSkill.put("Struts", "Struts");

		return javaSkill;
	}

	@ModelAttribute("countryList")
	public Map<String,String> populateCountryList() {

		//Data referencing for java skills list box
		Map<String,String> country = new LinkedHashMap<String,String>();
		country.put("US", "United Stated");
		country.put("CHINA", "China");
		country.put("SG", "Singapore");
		country.put("MY", "Malaysia");

		return country;
	}
} 
```

要使注释工作，您必须在 Spring 中启用组件自动扫描特性。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">

	<context:component-scan base-package="com.mkyong.customer.controller" />

	<bean class="com.mkyong.customer.validator.CustomerValidator" />

 	<!-- Register the Customer.properties -->
	<bean id="messageSource"
		class="org.springframework.context.support.ResourceBundleMessageSource">
		<property name="basename" value="com/mkyong/customer/properties/Customer" />
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

## 下载源代码

Download it – [SpringMVC-Form-Handling-Annotation-Example.zip](http://web.archive.org/web/20200614235038/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-Form-Handling-Annotation-Example.zip) (12KB)

## 参考

1.  [spring 2.5 中带注释的 web mvc 控制器](http://web.archive.org/web/20200614235038/http://blog.springsource.com/2007/11/14/annotated-web-mvc-controllers-in-spring-25/)
2.  [Spring MVC 表单处理示例–XML 版本](http://web.archive.org/web/20200614235038/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-example/)
3.  [Spring MVC hello world 注释示例](http://web.archive.org/web/20200614235038/http://www.mkyong.com/spring-mvc/spring-mvc-hello-world-annotation-example/)
4.  [Spring MVC multi action controller 注释示例](http://web.archive.org/web/20200614235038/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-annotation-example/)

Tags : [annotation](http://web.archive.org/web/20200614235038/https://mkyong.com/tag/annotation/) [form](http://web.archive.org/web/20200614235038/https://mkyong.com/tag/form/) [spring mvc](http://web.archive.org/web/20200614235038/https://mkyong.com/tag/spring-mvc/)<input type="hidden" id="mkyong-current-postId" value="6799">