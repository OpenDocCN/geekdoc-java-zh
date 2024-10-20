# Struts <checkbox>复选框示例</checkbox>

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-htmlcheckbox-checkbox-example/>

Download this Struts check box example – [Struts-CheckBox-Example.zip](http://web.archive.org/web/20190223075228/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-CheckBox-Example.zip)

在这个 Struts 示例中，您将学习如何创建一个带有 Struts**<HTML:checkbox>**标签的 HTML 复选框输入字段

## 1.文件夹结构

这是 Maven 创建的最终项目结构。请创建相应的文件夹。

![](img/90442f5cb5ad8eb614f4cedc388b1f0f.png "Struts-check-box-folder") ## 2.动作类

创建一个 Action 类，除了转发请求什么也不做。

**HtmlCheckBoxAction.java**

```java
 package com.mkyong.common.action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.struts.action.Action;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;

public class HtmlCheckBoxAction extends Action{

	public ActionForward execute(ActionMapping mapping,ActionForm form,
			HttpServletRequest request,HttpServletResponse response) 
        throws Exception {

		return mapping.findForward("success");
	}

} 
```

 ## 3.属性文件

创建一个属性文件，并声明错误和标签消息。

**公共属性**

```java
 #error message
error.common.html.checkbox.required = Please tick the checkbox.

#label message
label.common.html.checkbox.name = CheckBox
label.common.html.checkbox.button.submit = Submit
label.common.html.checkbox.button.reset = Reset 
```

## 4.动作形式

创建一个 ActionForm，接受一个复选框值。

**HtmlCheckBoxForm.java**

```java
 package com.mkyong.common.form;

import javax.servlet.http.HttpServletRequest;

import org.apache.struts.action.ActionErrors;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionMapping;
import org.apache.struts.action.ActionMessage;

public class HtmlCheckBoxForm extends ActionForm{

	String checkboxValue;

	public String getCheckboxValue() {
		return checkboxValue;
	}

	public void setCheckboxValue(String checkboxValue) {
		this.checkboxValue = checkboxValue;
	}

	@Override
	public ActionErrors validate(ActionMapping mapping,
			HttpServletRequest request) {

	    ActionErrors errors = new ActionErrors();

	    if( getCheckboxValue() == null || ("".equals(getCheckboxValue()))) {
	       errors.add("common.checkbox.err",
	    	  new ActionMessage("error.common.html.checkbox.required"));
	    }

	    return errors;
	}

	@Override
	public void reset(ActionMapping mapping, HttpServletRequest request) {
		// reset properties
		checkboxValue = "";
	}

} 
```

## 5.JSP 页面

使用 Struts 的 html 标签 **< html:checkbox >** 创建一个 HTML 复选框输入字段。

**checkbox.jsp**

```java
<%@taglib uri="http://struts.apache.org/tags-html" prefix="html"%>
<%@taglib uri="http://struts.apache.org/tags-bean" prefix="bean"%>

Struts html:复选框示例

```

<form action="/CheckBox"><messages id="err_name" property="common.checkbox.err"></messages><message key="label.common.html.checkbox.name">:</message><submit><message key="label.common.html.checkbox.button.submit"></message></submit><reset><message key="label.common.html.checkbox.button.reset"></message></reset></form>

显示复选框值。

**display.jsp**

```java
<%@taglib uri="http://struts.apache.org/tags-bean" prefix="bean"%>

复选框值:

```

## 6.struts-config.xml

创建一个 Struts 配置文件，并将它们链接在一起。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<form-beans>
		<form-bean
			name="htmlCheckBoxForm"
			type="com.mkyong.common.form.HtmlCheckBoxForm"/>

	</form-beans>

	<action-mappings>

	        <action
			path="/CheckBoxPage"
			type="org.apache.struts.actions.ForwardAction"
			parameter="/pages/checkbox.jsp"/>

		<action
			path="/CheckBox"
			type="com.mkyong.common.action.HtmlCheckBoxAction"
			name="htmlCheckBoxForm"
			validate="true"
			input="/pages/checkbox.jsp"
			>	

			<forward name="success" path="/pages/display.jsp"/>
		</action>

	</action-mappings>

	<message-resources
		parameter="com.mkyong.common.properties.Common" />

</struts-config> 
```

## 7.web.xml

最后一步，为 Strut 框架集成创建一个 web.xml。

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Maven Struts Examples</display-name>

  <servlet>
    <servlet-name>action</servlet-name>
    <servlet-class>
        org.apache.struts.action.ActionServlet
    </servlet-class>
    <init-param>
        <param-name>config</param-name>
        <param-value>
         /WEB-INF/struts-config.xml
        </param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
       <servlet-name>action</servlet-name>
       <url-pattern>*.do</url-pattern>
  </servlet-mapping>

</web-app> 
```

访问它

> http://localhost:8080/struts example/checkbox page . do

![](img/4f77effd1e46d5a1fb68baea0db5230e.png "Struts-check-box-example1")

勾选复选框并按下提交按钮，它将转发到

> http://localhost:8080/struts example/checkbox . do

![](img/bb5f79f48ae44ccd092a897078a39abb.png "Struts-check-box-example2")

**如果复选框被选中，则值为“开”，否则为空值。**

[checkbox](http://web.archive.org/web/20190223075228/http://www.mkyong.com/tag/checkbox/) [struts](http://web.archive.org/web/20190223075228/http://www.mkyong.com/tag/struts/)







