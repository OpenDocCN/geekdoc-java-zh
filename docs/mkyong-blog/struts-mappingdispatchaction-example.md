# struts–MappingDispatchAction 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-mappingdispatchaction-example/>

Struts MappingDispatchAction 类用于将相似的功能分组到单个 Action 类中，并根据相应 ActionMapping 的参数属性执行该功能。下面的示例展示了 MappingDispatchAction 的用法。

Download this example – [Struts-MappingDispatchAction-Example.zip](http://web.archive.org/web/20190224170106/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-MappingDispatchAction-Example.zip)

## 1.MappingDispatchAction 类

扩展 MappingDispatchAction 类，并声明两个方法—**generate XML()**和 **generateExcel()** 。

**MyCustomDispatchAction.java**

```java
 package com.mkyong.common.action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.apache.struts.actions.MappingDispatchAction;

public class MyCustomDispatchAction extends MappingDispatchAction{

	public ActionForward generateXML(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
        throws Exception {

		request.setAttribute("method", "generateXML is called");

	        return mapping.findForward("success");
	}

	public ActionForward generateExcel(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
	throws Exception {

		request.setAttribute("method", "generateExcel is called");

		return mapping.findForward("success");
	}
} 
```

 ## 2.Struts 配置

声明两个操作映射，每个映射指向具有不同参数属性的同一个 MyCustomDispatchAction 类。

**struts-config.xml**

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<action-mappings>

	 	<action
			path="/CustomDispatchActionXML"
			type="com.mkyong.common.action.MyCustomDispatchAction"
			parameter="generateXML"
			>

			<forward name="success" path="/pages/DispatchExample.jsp"/>

		</action>

		<action
			path="/CustomDispatchActionExcel"
			type="com.mkyong.common.action.MyCustomDispatchAction"
			parameter="generateExcel"
			>

			<forward name="success" path="/pages/DispatchExample.jsp"/>

		</action>

		<action
			path="/Test"
			type="org.apache.struts.actions.ForwardAction"
			parameter="/pages/TestForm.jsp"
			>
		</action>

	</action-mappings>

</struts-config> 
```

 ## 3.查看页面

在 JSP 页面中，链接的工作方式如下:

1. **/CustomDispatchActionXML** 将执行 **generateXML()** 方法。
2。**/CustomDispatchActionExcel**将执行 **generateExcel()** 方法。

**TestForm.jsp**

```java
<%@taglib uri="http://struts.apache.org/tags-html" prefix="html"%>

Struts - DispatchAction 示例

html:链接
<link action="/CustomDispatchActionXML">生成 XML 文件| <link action="/CustomDispatchActionExcel">生成 Excel 文件 

a href
[生成 XML 文件](CustomDispatchActionXML.do) | [生成 Excel 文件](CustomDispatchActionExcel.do) 

```

**DispatchExample.jsp**

```java
<%@taglib uri="http://struts.apache.org/tags-bean" prefix="bean"%>
<%@taglib uri="http://struts.apache.org/tags-logic" prefix="logic"%>

Struts - DispatchAction 示例

```

## 4.测试一下

*http://localhost:8080/struts example/test . do*

![Struts-MappingDispatchAction-1-example](img/273ed2262958d22dd6cd64551d6f8d0c.png "Struts-MappingDispatchAction-1-example")

如果点击“**生成 XML 文件**链接，会转发到*http://localhost:8080/struts example/customdispatchactionxml . do*

![Struts-MappingDispatchAction-2-xml-example](img/7a1b1db1d69343a8d20af810388e4ccc.png "Struts-MappingDispatchAction-2-xml-example")

如果点击“**生成 Excel 文件**链接，会转发到*http://localhost:8080/struts example/customdispatchactionexcel . do*

![Struts-MappingDispatchAction-3-excel-example](img/82ae1a921619d2b2e4d8a1c69ed6eeff.png "Struts-MappingDispatchAction-3-excel-example")[struts](http://web.archive.org/web/20190224170106/http://www.mkyong.com/tag/struts/)







