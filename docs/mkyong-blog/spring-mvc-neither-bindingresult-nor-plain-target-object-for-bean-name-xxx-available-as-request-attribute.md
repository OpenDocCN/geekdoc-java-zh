# spring MVC–bean 名称“xxx”的 BindingResult 和 plain target 对象都不能用作请求属性。

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-neither-bindingresult-nor-plain-target-object-for-bean-name-xxx-available-as-request-attribute/>

## 问题

最近刚刚将 [Spring MVC 基于 xml 的表单控制器](http://web.archive.org/web/20190225092853/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-example/)转换为[基于注释的表单控制器](http://web.archive.org/web/20190225092853/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-annotation-example/)，出现如下错误信息。
 <font color="red">*严重:bean 名称“customerForm”的 BindingResult 和 plain target 对象都不能用作请求属性
Java . lang . illegalstateexception:bean 名称“customerForm”的 BindingResult 和 plain target 对象都不能用作请求属性*</font> 
上述错误消息清楚地表明“customer form”bean 不存在，并且我 100%确定视图解析器配置正确，并且“CustomerForm.jsp”视图页面存在。

*表单控制器*

```java
 @Controller
@RequestMapping("/customer.htm")
public class CustomerController{

       @RequestMapping(method = RequestMethod.GET)
	public String initForm(ModelMap model){
		//return form view
		return "CustomerForm";
	} 
```

*查看解析器*

```java
 ...
	<bean id="viewResolver"
	      class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
              <property name="prefix">
                  <value>/WEB-INF/pages/</value>
              </property>
              <property name="suffix">
                 <value>.jsp</value>
             </property>
        </bean> 
```

 ## 解决办法

造成这种情况的根本原因是 JSP 页面中的视图名称不正确，见下文。

```java
 <form:form method="POST" commandName="customerForm"> 
```

“customerForm”不再存在于控制器映射中，请参见注释映射**@ request mapping("/customer . htm ")**，它应该更改为“customer”。

```java
 <form:form method="POST" commandName="customer"> 
```

 ## 类似案例

我在 validator 或 SimpleFormController 类中也见过很多类似的情况。要解决它，只需确保映射名称匹配或存在。

[spring mvc](http://web.archive.org/web/20190225092853/http://www.mkyong.com/tag/spring-mvc/)







