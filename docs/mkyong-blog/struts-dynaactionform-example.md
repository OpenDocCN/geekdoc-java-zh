# Struts DynaActionForm 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-dynaactionform-example/>

Struts **DynaActionForm** 类是一个有趣的特性，它允许您以动态和声明的方式创建表单 bean。它使您能够在 Struts 配置文件中创建一个“虚拟”表单 bean，而不是创建一个真正的 Java 表单 bean 类。它可以避免您创建许多简单但繁琐的表单 bean 类。

例如，DynaActionForm 包含一个“用户名”属性。

```java
 <form-bean name="dynaUserForm"   
	type="org.apache.struts.action.DynaActionForm">
    <form-property name="username" type="java.lang.String"/>
</form-bean> 
```

“DynaActionForm”和“ActionForm”的区别

*   创建真正的 Java 类不需要 DynaActionForm(只需在 Struts 配置文件中声明)，但 ActionForm 需要。
*   在 DynaActionForm 中，表单验证是在 Action 类中实现，而 ActionForm 是在自己的类中实现的。

## DynaActionForm 示例

[Struts<html:text>textbox 示例](http://web.archive.org/web/20200417115557/http://www.mkyong.com/struts/struts-htmltext-textbox-example/)将被重构为使用“DynaActionForm”而不是普通的“ActionForm”。

Download Struts DynaActionForm example – [Struts-DynaActionForm-Example.zip](http://web.archive.org/web/20200417115557/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-DynaActionForm-Example.zip)

## 1.struts-config.xml

在 Struts 配置文件中声明“DynaActionForm ”,并像 normal 一样将其链接到 Action 类。

**struts-config.xml**

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<form-beans>
		<!--<form-bean
			name="userForm"
			type="com.mkyong.common.form.UserForm"/>
		-->
		<form-bean name="dynaUserForm"   
		      type="org.apache.struts.action.DynaActionForm">
		      <form-property name="username" type="java.lang.String"/>
		</form-bean>

	</form-beans>

	<action-mappings>

	        <action
			path="/LoginPage"
			type="org.apache.struts.actions.ForwardAction"
			parameter="/pages/login.jsp"/>

		<action
			path="/Login"
			type="com.mkyong.common.action.UserAction"
			name="dynaUserForm"
			>	

			<forward name="success" path="/pages/welcome.jsp"/>
			<forward name="failed" path="/pages/login.jsp"/>

		</action>
	</action-mappings>

	<message-resources
		parameter="com.mkyong.common.properties.Common" />

</struts-config> 
```

## 2.行动

将所有表单验证方法移动到 Action 类中，通过“ **get()** ”方法获取“DynaActionForm”属性。

user action . Java

```java
 package com.mkyong.common.action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.struts.action.Action;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.apache.struts.action.ActionMessage;
import org.apache.struts.action.ActionMessages;
import org.apache.struts.action.DynaActionForm;

public class UserAction extends Action{

	public ActionForward execute(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
        throws Exception {

		DynaActionForm userForm = (DynaActionForm)form;

		ActionMessages errors = new ActionMessages();

		//do the form validation in action class
	    if( userForm.get("username") == null || 
                         ("".equals(userForm.get("username")))) {

	       errors.add("common.name.err",
                    new ActionMessage("error.common.name.required"));

	    }

	    saveErrors(request,errors);

	    if(errors.isEmpty()){
	        return mapping.findForward("success");
	    }else{
	        return mapping.findForward("failed");
	    }

	}

} 
```

## 结论

你应该选择 DynaActionForm 吗？此功能可以节省您创建 ActionForm 类的大量时间，但是它有局限性，有时您必须使用真正的 ActionForm 来完成某些任务。在大型项目环境中，维护总是要考虑的第一优先事项，您必须创建一个“表单标准”来遵循，混合使用这两者是不实际的，除非您有非常坚实的理由来支持。就我个人而言，我很少使用 DynaActionForm，有了 Eclipse IDE，ActionForm 毕竟不是很难创建。

[hello bots](http://web.archive.org/web/20200417115557/https://mkyong.com/hello-bots)[struts](http://web.archive.org/web/20200417115557/https://mkyong.com/tag/struts/)<input type="hidden" id="mkyong-current-postId" value="4579">