# Struts 2 附加标签示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-append-tag-example/>

Download It – [Struts2-Append-Tag-Example.zip](http://web.archive.org/web/20190222152717/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-Append-Tag-Example.zip)

Struts 2 **append** 标签用于将几个迭代器(由 List 或 Map 创建)组合成一个迭代器。在本教程中，您将使用 Struts 2 **append** 标签来完成以下任务:

1.  将三个数组列表组合成一个迭代器。
2.  将三个散列表组合成一个迭代器。
3.  将 **ArrayList** 和 **HashMap** 组合成一个迭代器。

Assume 2 iterators, each has two entries, after combine with **append** tag into a single iterator, the order of the entries will look like following :

1.  第一个迭代器的第一个条目。
2.  第一个迭代器的第二个条目。
3.  第二个迭代器的第一个条目。
4.  第二个迭代器的第二个条目。

这只适用于列表迭代器；地图迭代器，顺序会随机。

## 1.行动

一个具有 3 个 ArrayList 和 3 个 HashMap 属性的 Action 类。

**追加动作**

```java
 package com.mkyong.common.action;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import com.opensymphony.xwork2.ActionSupport;

public class AppendTagAction extends ActionSupport{

	private List<String> list1 = new ArrayList<String>();
	private List<String> list2 = new ArrayList<String>();
	private List<String> list3 = new ArrayList<String>();

	private Map<String,String> map1 = new HashMap<String,String>();
	private Map<String,String> map2 = new HashMap<String,String>();
	private Map<String,String> map3 = new HashMap<String,String>();

	public String execute() {

		list1.add("List1 - 1");
		list1.add("List1 - 2");
		list1.add("List1 - 3");

		list2.add("List2 - 1");
		list2.add("List2 - 2");
		list2.add("List2 - 3");

		list3.add("List3 - 1");
		list3.add("List3 - 2");
		list3.add("List3 - 3");

		map1.put("map1-key1", "map1-value1");
		map1.put("map1-key2", "map1-value2");
		map1.put("map1-key3", "map1-value3");

		map2.put("map2-key1", "map2-value1");
		map2.put("map2-key2", "map2-value2");
		map2.put("map2-key3", "map2-value3");

		map3.put("map3-key1", "map3-value1");
		map3.put("map3-key2", "map3-value2");
		map3.put("map3-key3", "map3-value3");

		return SUCCESS;
	}

	//getter methods...
} 
```

 ## 2.附加标签示例

一个 JSP 页面，展示了如何使用 **append** 标记将 3 ArrayList/3 HashMap/1 ArrayList+1 HashMap 组合成一个迭代器，并循环遍历其值并打印出来。

**appendIterator.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
 <html>
<head>
</head>

<body>
<h1>Struts 2 Append tag example</h1>

1\. Combine 3 ArrayList into a single iterator.
<s:append var="customListIterator">
     <s:param value="%{list1}" />
     <s:param value="%{list2}" />
     <s:param value="%{list3}" />
</s:append>
<ol>
<s:iterator value="%{#customListIterator}">
     <li><s:property /></li>
</s:iterator>
</ol>

2\. Combine 3 HashMap into a single iterator.
<s:append var="customMapIterator">
     <s:param value="%{map1}" />
     <s:param value="%{map2}" />
     <s:param value="%{map3}" />
</s:append>
<ol>
<s:iterator value="%{#customMapIterator}">
     <li><s:property /></li>
</s:iterator>
</ol>

3\. Combine ArrayList and HashMap into a single iterator.
<s:append var="customMixedIterator">
     <s:param value="%{list1}" />
     <s:param value="%{map1}" />
</s:append>
<ol>
<s:iterator value="%{#customMixedIterator}">
     <li><s:property /></li>
</s:iterator>
</ol>

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

		<action name="appendTagAction" 
			class="com.mkyong.common.action.AppendTagAction" >
			<result name="success">pages/appendIterator.jsp</result>
		</action>

	</package>

</struts> 
```

## 4.演示

*http://localhost:8080/struts 2 example/appendtagaction . action*

**输出**

```java
 Struts 2 Append tag example

1\. Combine 3 ArrayList into a single iterator.

  1\. List1 - 1
  2\. List1 - 2
  3\. List1 - 3
  4\. List2 - 1
  5\. List2 - 2
  6\. List2 - 3
  7\. List3 - 1
  8\. List3 - 2
  9\. List3 - 3

2\. Combine 3 HashMap into a single iterator.

  1\. map1-key3=map1-value3
  2\. map1-key1=map1-value1
  3\. map1-key2=map1-value2
  4\. map2-key2=map2-value2
  5\. map2-key3=map2-value3
  6\. map2-key1=map2-value1
  7\. map3-key3=map3-value3
  8\. map3-key1=map3-value1
  9\. map3-key2=map3-value2

3\. Combine ArrayList and HashMap into a single iterator.

  1\. List1 - 1
  2\. List1 - 2
  3\. List1 - 3
  4\. map1-key3=map1-value3
  5\. map1-key1=map1-value1
  6\. map1-key2=map1-value2 
```

## 参考

1.  [Struts 2 追加标签文档](http://web.archive.org/web/20190222152717/http://struts.apache.org/2.1.8/docs/append.html)

[struts2](http://web.archive.org/web/20190222152717/http://www.mkyong.com/tag/struts2/)







