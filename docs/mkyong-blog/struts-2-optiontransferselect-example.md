# Struts 2 选项转移选择示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-optiontransferselect-example/>

Download It – [Struts2-OptionTransferSelect-Example.zip](http://web.archive.org/web/20190304032237/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-OptionTransferSelect-Example.zip)

在 Struts 2 中，选项传递选择组件是两个" **updownselect** "选择组件左右对齐，在它们的中间，包含在它们之间移动选择选项的按钮。这可以通过**<s:optiontransferselect>**标签创建。

```java
 <s:optiontransferselect
     label="Lucky Numbers"
     name="leftNumber"
     list="{'1 - One ', '2 - Two', '3 - Three', '4 - Four', '5 - Five'}"
     doubleName="rightNumber"
     doubleList="{'10 - Ten','20 - Twenty','30 - Thirty','40 - Forty','50 - Fifty'}"
 /> 
```

The “**name**” and “**list**” is refer to the left select component; while the “**doubleName**” and “**doubleList**” is refer to the right select component.

结果是下面的 HTML，两个" **updownselect** "组件，按钮和 JavaScript 在它们之间移动选择选项(默认的 xhtml 主题)。

```java
 <tr> 
<td class="tdLabel">
<label for="resultAction_leftNumber" class="label">Lucky Numbers:</label>
</td> 
<td>
<script type="text/javascript" src="/Struts2Example/struts/optiontransferselect.js">
</script> 
<table border="0"> 
<tr> 
<td> 
<select name="leftNumber" size="15" 
id="resultAction_leftNumber" multiple="multiple"> 
    <option value="1 - One ">1 - One </option> 
    <option value="2 - Two">2 - Two</option> 
    <option value="3 - Three">3 - Three</option> 
    <option value="4 - Four">4 - Four</option> 
    <option value="5 - Five">5 - Five</option> 
</select> 
<input type="hidden" id="__multiselect_resultAction_leftNumber" 
name="__multiselect_leftNumber" value="" /> 
<input type="button"
	onclick="moveOptionDown(
        document.getElementById('resultAction_leftNumber'), 'key', '');"
	value="v"
/> 
<input type="button"
	onclick="moveOptionUp(
        document.getElementById('resultAction_leftNumber'), 'key', '');"
	value="^"
/> 
</td> 
<td valign="middle" align="center"> 
<input type="button"
    value="<-" onclick="moveSelectedOptions(
    document.getElementById('resultAction_rightNumber'), 
    document.getElementById('resultAction_leftNumber'), false, '');" />
<input type="button"
    value="->" onclick="moveSelectedOptions(
    document.getElementById('resultAction_leftNumber'), 
    document.getElementById('resultAction_rightNumber'), false, '');" /> 
<input type="button"
    value="<<--" onclick="moveAllOptions(
    document.getElementById('resultAction_rightNumber'), 
    document.getElementById('resultAction_leftNumber'), false, '');" />
<input type="button"
    value="-->>" onclick="moveAllOptions(
    document.getElementById('resultAction_leftNumber'), 
    document.getElementById('resultAction_rightNumber'), false, '');" />
<input type="button"
    value="<*>" onclick="selectAllOptions(
    document.getElementById('resultAction_leftNumber'));
    selectAllOptions(document.getElementById('resultAction_rightNumber'));" />
</td> 
<td> 
<select 
	name="rightNumber"
	size="15"
	multiple="multiple"
	id="resultAction_rightNumber"
> 
    	<option value="10 - Ten">10 - Ten</option> 
    	<option value="20 - Twenty">20 - Twenty</option> 
    	<option value="30 - Thirty">30 - Thirty</option> 
    	<option value="40 - Forty">40 - Forty</option> 
    	<option value="50 - Fifty">50 - Fifty</option> 
</select> 
<input type="hidden" id="__multiselect_resultAction_rightNumber" 
name="__multiselect_rightNumber" value="" /> 
<input type="button"
   onclick="moveOptionDown(
   document.getElementById('resultAction_rightNumber'), 'key', '');"
   value="v"
/> 
<input type="button"
   onclick="moveOptionUp(
   document.getElementById('resultAction_rightNumber'), 'key', '');"
   value="^"
/> 
</td> 
</tr> 
</table> 

<script type="text/javascript"> 
var containingForm = document.getElementById("resultAction");
StrutsUtils.addEventListener(containingForm, "submit", 
  function(evt) {
	var selectObj = document.getElementById("resultAction_leftNumber");
		selectAllOptionsExceptSome(selectObj, "key", "");
  }, true);
var containingForm = document.getElementById("resultAction");
StrutsUtils.addEventListener(containingForm, "submit", 
  function(evt) {
	var selectObj = document.getElementById("resultAction_rightNumber");
		selectAllOptionsExceptSome(selectObj, "key", "");
	}, true);
</script> 
```

## Struts 2 <optiontransferselect>示例</optiontransferselect>

**<s:optiontransferselect>**标签的完整示例，展示了使用 OGNL 和 Java 列表将数据填充到“optiontransferselect”组件中。

 ## 1.动作类

Action 类来生成和存储左右选择选项。

**OptionTransferSelectAction.java**

```java
 package com.mkyong.common.action;

import java.util.ArrayList;
import java.util.List;

import com.opensymphony.xwork2.ActionSupport;

public class OptionTransferSelectAction extends ActionSupport{

	private List<String> leftAntivirusList = new ArrayList<String>();
	private List<String> rightAntivirusList = new ArrayList<String>();

	private String leftAntivirus;
	private String rightAntivirus;

	private String leftNumber;
	private String rightNumber;

	public OptionTransferSelectAction(){

		leftAntivirusList.add("Norton 360 Version 4.0");
		leftAntivirusList.add("McAfee Total Protection 2010");
		leftAntivirusList.add("Trend Micro IS Pro 2010");
		leftAntivirusList.add("BitDefender Total Security 2010");

		rightAntivirusList.add("Norton Internet Security 2010");
		rightAntivirusList.add("Kaspersky Internet Security 2010");
		rightAntivirusList.add("McAfee Internet Security 2010");
		rightAntivirusList.add("AVG Internet Security 2010");
		rightAntivirusList.add("Trend Micro Internet Security 2010");
		rightAntivirusList.add("F-Secure Internet Security 2010");

	}

	public String getLeftNumber() {
		return leftNumber;
	}

	public void setLeftNumber(String leftNumber) {
		this.leftNumber = leftNumber;
	}

	public String getRightNumber() {
		return rightNumber;
	}

	public void setRightNumber(String rightNumber) {
		this.rightNumber = rightNumber;
	}

	public List<String> getLeftAntivirusList() {
		return leftAntivirusList;
	}

	public void setLeftAntivirusList(List<String> leftAntivirusList) {
		this.leftAntivirusList = leftAntivirusList;
	}

	public List<String> getRightAntivirusList() {
		return rightAntivirusList;
	}

	public void setRightAntivirusList(List<String> rightAntivirusList) {
		this.rightAntivirusList = rightAntivirusList;
	}

	public String getLeftAntivirus() {
		return leftAntivirus;
	}

	public void setLeftAntivirus(String leftAntivirus) {
		this.leftAntivirus = leftAntivirus;
	}

	public String getRightAntivirus() {
		return rightAntivirus;
	}

	public void setRightAntivirus(String rightAntivirus) {
		this.rightAntivirus = rightAntivirus;
	}

	public String execute() throws Exception{

		return SUCCESS;
	}

	public String display() {
		return NONE;
	}

} 
```

 ## 2.结果页面

通过“**<s:optiontransferselect>**”标签渲染选项转移选择组件，通过 Java 和 OGNL 列表生成左右选择选项。

**option transfer elect . JSP**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
<s:head />
</head>

<body>
<h1>Struts 2 optiontransferselect example</h1>

<s:form action="resultAction" namespace="/" method="POST" >

<s:optiontransferselect
     label="Lucky Numbers"
     name="leftNumber"
     list="{'1 - One ', '2 - Two', '3 - Three', '4 - Four', '5 - Five'}"
     doubleName="rightNumber"
     doubleList="{'10 - Ten','20 - Twenty','30 - Thirty','40 - Forty','50 - Fifty'}"
 />

<s:optiontransferselect
     label="Favourite Antivirus"
     name="leftAntivirus"
     leftTitle="Left Antivirus Title"
     rightTitle="Right Antivirus Title"
     list="leftAntivirusList"
     multiple="true"
     headerKey="-1"
     headerValue="--- Please Select ---"
     doubleList="rightAntivirusList"
     doubleName="rightAntivirus"
     doubleHeaderKey="-1"
     doubleHeaderValue="--- Please Select ---"
 />

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
<h1>Struts 2 optiontransferselect example</h1>

<h2>
   Left AntiVirus : <s:property value="leftAntivirus"/> 
</h2> 

<h2>
   Right AntiVirus : <s:property value="rightAntivirus"/> 
</h2> 

<h2>
   Left Numbers : <s:property value="leftNumber"/> 
</h2> 

<h2>
   Right Numbers : <s:property value="rightNumber"/> 
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

  <action name="optionTransferSelectAction" 
	class="com.mkyong.common.action.OptionTransferSelectAction" 
        method="display">
	<result name="none">pages/optiontransferselect.jsp</result>
  </action>

  <action name="resultAction" 
        class="com.mkyong.common.action.OptionTransferSelectAction" >
	<result name="success">pages/result.jsp</result>
  </action>
</package>

</struts> 
```

## 4.演示

*http://localhost:8080/struts 2 example/optiontransferselectaction . action*

![Struts 2 Option Transfer Select example](img/bae09081ddcadfee537951aae95f0222.png "Struts2-Option-Transfer-Select-Example-1")![Struts 2 Option Transfer Select example](img/7d339a2463052af1bcf82a9b0c0f9e1b.png "Struts2-Option-Transfer-Select-Example-2")

## 参考

1.  [Struts 2 updownselect 文档](http://web.archive.org/web/20190304032237/http://struts.apache.org/2.0.14/docs/updownselect.html)
2.  [Struts 2 updownselect 示例](http://web.archive.org/web/20190304032237/http://www.mkyong.com/struts2/struts-2-updownselect-example/)
3.  [Struts 2 双重选择示例](http://web.archive.org/web/20190304032237/http://www.mkyong.com/struts2/struts-2-sdoubleselect-example/)

[struts2](http://web.archive.org/web/20190304032237/http://www.mkyong.com/tag/struts2/)







