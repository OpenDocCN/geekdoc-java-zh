# Struts 2 日期时间选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-datetimepicker-example/>

Download It – [Struts2-DateTimePicker-Example.zip](http://web.archive.org/web/20190304031544/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-DateTimePicker-Example.zip)

在 Struts 2 中，dojo ajax 标签“**<sx:datetime picker>**”会呈现一个文本框并在后面追加一个日历图标，点击日历图标会提示一个日期时间选择器组件。

**创建一个日期时间选择组件，只需确保:**
1。下载 struts2-dojo-plugin.jar 库。
2。包括“struts-dojo-tags”标记，并输出其标题。

```java
 <%@ taglib prefix="sx" uri="/struts-dojo-tags" %>
<html>
<head>
<sx:head />
</head> 
```

举个例子，

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<%@ taglib prefix="sx" uri="/struts-dojo-tags" %>
<html>
<head>
<sx:head />
</head>
<body>
<sx:datetimepicker name="date2" label="Format (dd-MMM-yyyy)" 
displayFormat="dd-MMM-yyyy" value="%{'2010-01-01'}"/>
... 
```

产生了下面的 HTML，几个 dojo 和 JavaScript 库来创建一个日期时间选择组件。

```java
 <html> 
<head> 
<script language="JavaScript" type="text/javascript"> 
    // Dojo configuration
    djConfig = {
        isDebug: false,
        bindEncoding: "UTF-8"
          ,baseRelativePath: "/Struts2Example/struts/dojo/"
          ,baseScriptUri: "/Struts2Example/struts/dojo/"
         ,parseWidgets : false
    };
</script> 
<script language="JavaScript" type="text/javascript"
        src="/Struts2Example/struts/dojo/struts_dojo.js"></script> 

<script language="JavaScript" type="text/javascript"
        src="/Struts2Example/struts/ajax/dojoRequire.js"></script> 

<link rel="stylesheet" href="/Struts2Example/struts/xhtml/styles.css" type="text/css"/> 

<script language="JavaScript" src="/Struts2Example/struts/utils.js" 
type="text/javascript"></script> 

<script language="JavaScript" src="/Struts2Example/struts/xhtml/validation.js" 
type="text/javascript"></script> 

<script language="JavaScript" src="/Struts2Example/struts/css_xhtml/validation.js" 
type="text/javascript"></script> 
</head> 
...
<td class="tdLabel">
<label for="widget_1291193434" class="label">Format (dd-MMM-yyyy):
</label></td> 
<td>
<div dojoType="struts:StrutsDatePicker"    id="widget_1291193434"    
value="2010-01-01"    name="date2"    inputName="dojo.date2"    
displayFormat="dd-MMM-yyyy"  saveFormat="rfc"></div> 
</td> 
</tr> 
<script language="JavaScript" type="text/javascript">
djConfig.searchIds.push("widget_1291193434");</script> 
```

## Struts 2 <datetimepicker>示例</datetimepicker>

一个完整的完整示例，使用 **< s:datetimepicker >** 标签生成一个 **datetimepicker** 组件，并展示如何使用 OGNL 和 Java 属性将默认日期设置为“ **datetimepicker** ”组件。

 ## 1.pom.xml

下载 Struts 2 dojo 依赖库。

**pom.xml**

```java
 //...
   <!-- Struts 2 -->
   <dependency>
      <groupId>org.apache.struts</groupId>
      <artifactId>struts2-core</artifactId>
      <version>2.1.8</version>
    </dependency>

    <!-- Struts 2 Dojo Ajax Tags -->
    <dependency>
      <groupId>org.apache.struts</groupId>
      <artifactId>struts2-dojo-plugin</artifactId>
      <version>2.1.8</version>
    </dependency>
//... 
```

 ## 2.动作类

存储选定日期的操作类。

**date time pick . Java**

```java
 package com.mkyong.common.action;

import java.util.Date;
import com.opensymphony.xwork2.ActionSupport;

public class DateTimePickerAction extends ActionSupport{

	private Date date1;
	private Date date2;
	private Date date3;

	//return today date
	public Date getTodayDate(){

		return new Date();
	}

	//getter and setter methods
	public String execute() throws Exception{

		return SUCCESS;
	}

	public String display() {
		return NONE;
	}

} 
```

## 3.结果页面

通过“ **< s:datetimepicker >** ”标签呈现日期时间选择器组件，通过 Java 属性和 OGNL 设置默认日期。

The ‘**displayFormat**‘ attribute is supported in many date patterns, read this article – [Date Format Patterns](http://web.archive.org/web/20190304031544/http://www.unicode.org/reports/tr35/tr35-4.html#Date_Format_Patterns).Ensure you put the “struts-dojo-tags” tag and render it’s header <sx:head />

```java
 <%@ taglib prefix="sx" uri="/struts-dojo-tags" %>
<html>
<head>
<sx:head />
</head> 
```

**datetimepicker.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<%@ taglib prefix="sx" uri="/struts-dojo-tags" %>
<html>
<head>
<sx:head />
</head>

<body>
<h1>Struts 2 datetimepicker example</h1>

<s:form action="resultAction" namespace="/" method="POST" >

<sx:datetimepicker name="date1" label="Format (dd-MMM-yyyy)" 
displayFormat="dd-MMM-yyyy" value="todayDate" />

<sx:datetimepicker name="date2" label="Format (dd-MMM-yyyy)" 
displayFormat="dd-MMM-yyyy" value="%{'2010-01-01'}"/>

<sx:datetimepicker name="date3" label="Format (dd-MMM-yyyy)" 
displayFormat="dd-MMM-yyyy" value="%{'today'}"/>

<s:submit value="submit" name="submit" />

</s:form>

</body>
</html> 
```

**result.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>

<body>
<h1>Struts 2 datetimepicker example</h1>

<h2>
   Date1 : <s:property value="date1"/> 
</h2> 

<h2>
   Date 2 : <s:property value="date2"/> 
</h2> 

<h2>
   Date 3 : <s:property value="date3"/> 
</h2> 

</body>
</html> 
```

## 3.struts.xml

全部链接起来~

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>

 <constant name="struts.devMode" value="true" />

<package name="default" namespace="/" extends="struts-default">

  <action name="dateTimePickerAction" 
	class="com.mkyong.common.action.DateTimePickerAction" 
        method="display">
	<result name="none">pages/datetimepicker.jsp</result>
  </action>

  <action name="resultAction" 
        class="com.mkyong.common.action.DateTimePickerAction" >
	<result name="success">pages/result.jsp</result>
  </action>
</package>

</struts> 
```

## 4.演示

*http://localhost:8080/struts 2 example/datetimepickeraction . action*

![Struts 2 datetimepicker example](img/80f4b6c90b6b0c580f5d8c3fa82946c1.png "Struts2-DateTimePicker-Example-1")![Struts 2 datetimepicker example](img/1792631d0c09cd335b520d60cea3f00d.png "Struts2-DateTimePicker-Example-2")

## 参考

1.  [Struts 2 datetimepicker 文档](http://web.archive.org/web/20190304031544/http://struts.apache.org/2.0.14/docs/datetimepicker.html)
2.  [Struts 2 ajax 和 javascript 配方](http://web.archive.org/web/20190304031544/http://struts.apache.org/2.0.14/docs/ajax-and-javascript-recipes.html)
3.  [日期格式模式](http://web.archive.org/web/20190304031544/http://www.unicode.org/reports/tr35/tr35-4.html#Date_Format_Patterns)

[date](http://web.archive.org/web/20190304031544/http://www.mkyong.com/tag/date/) [struts2](http://web.archive.org/web/20190304031544/http://www.mkyong.com/tag/struts2/)







