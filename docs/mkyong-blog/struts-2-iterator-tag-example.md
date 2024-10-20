# Struts 2 迭代器标签示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-iterator-tag-example/>

Download It – [Struts2-Iterator-tag-Example.zip](http://web.archive.org/web/20190304032825/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-Iterator-tag-Example.zip)

Struts 2 **迭代器标签**用于迭代一个值，可以是 **java.util.Collection** 或 **java.util.Iterator** 中的任意一个。在本教程中，您将创建一个列表变量，使用 Iterator 标记对其进行循环，并使用 **IteratorStatus** 获取迭代器状态。

## 1.行动

一个带有列表属性的动作类，包含各种美味的“肯德基套餐”。

**迭代器函数**

```java
 package com.mkyong.common.action;

import java.util.ArrayList;
import java.util.List;

import com.opensymphony.xwork2.ActionSupport;

public class IteratorKFCAction extends ActionSupport{

	private List<String> comboMeals;

	public List<String> getComboMeals() {
		return comboMeals;
	}

	public void setComboMeals(List<String> comboMeals) {
		this.comboMeals = comboMeals;
	}

	public String execute() {

		comboMeals = new ArrayList<String>();
		comboMeals.add("Snack Plate");
		comboMeals.add("Dinner Plate");
		comboMeals.add("Colonel Chicken Rice Combo");
		comboMeals.add("Colonel Burger");
		comboMeals.add("O.R. Fillet Burger");
		comboMeals.add("Zinger Burger");

		return SUCCESS;
	}
} 
```

 ## 2.迭代器示例

一个 JSP 页面，展示了如何使用**迭代器标签**来遍历“KFC comboMeals”列表。在**迭代器标签**中，包含一个“**状态**属性，用于声明**迭代器状态**类的名称。

The **IteratorStatus** class is used to get information about the status of the iteration. Supported properties are index, count, first, last, odd, even and etc..Make sure you visit this [IteratorStatus documentation](http://web.archive.org/web/20190304032825/http://struts.apache.org/2.1.8/struts2-core/apidocs/org/apache/struts2/views/jsp/IteratorStatus.html) to know more details of it.

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>

<html>
<head>
</head>

<body>
<h1>Struts 2 Iterator tag example</h1>

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Simple Iterator</h2>
<ol>
<s:iterator value="comboMeals">
  <li><s:property /></li>
</s:iterator>
</ol>

<h2>Iterator with IteratorStatus</h2>
<table>
<s:iterator value="comboMeals" status="comboMealsStatus">
  <tr>
  	<s:if test="#comboMealsStatus.even == true">
      <td style="background: #CCCCCC"><s:property/></td>
    </s:if>
    <s:elseif test="#comboMealsStatus.first == true">
      <td><s:property/> (This is first value) </td>
    </s:elseif>
    <s:else>
      <td><s:property/></td>
    </s:else>
  </tr>
</s:iterator>
</table>

</body>
</html> 
```

## 3.struts.xml

链接一下~

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>

 	<constant name="struts.devMode" value="true" />

	<package name="default" namespace="/" extends="struts-default">

		<action name="iteratorKFCAction" 
			class="com.mkyong.common.action.IteratorKFCAction" >
			<result name="success">pages/iterator.jsp</result>
		</action>

	</package>

</struts> 
```

## 4.演示

*http://localhost:8080/struts 2 example/iteratorkfcaction . action*

![Struts 2 Iterator tag ](img/c72e8661d633a1a0d4ef0bae3c869826.png "Struts2-Iterator-tag-example")

## 参考

1.  [Struts 2 迭代器标签示例](http://web.archive.org/web/20190304032825/http://struts.apache.org/2.0.14/docs/iterator.html)
2.  [迭代器状态文档](http://web.archive.org/web/20190304032825/http://struts.apache.org/2.1.8/struts2-core/apidocs/org/apache/struts2/views/jsp/IteratorStatus.html)

[iterator](http://web.archive.org/web/20190304032825/http://www.mkyong.com/tag/iterator/) [struts2](http://web.archive.org/web/20190304032825/http://www.mkyong.com/tag/struts2/)![](img/b50a69c32a54ffdd18b907d889ee1fa1.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304032825/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6086">







