# Struts 2 和 JSON 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-and-json-example/>

在 Struts 2 示例中，您将学习如何通过“`struts2-json-plugin.jar`”库将对象转换成 JSON 格式。

## 1.获取依赖库

获取 **struts2-json-plugin.jar** 库。
POM . XML

```java
 <!-- Struts 2 -->
    <dependency>
          <groupId>org.apache.struts</groupId>
	  <artifactId>struts2-core</artifactId>
	  <version>2.1.8</version>
    </dependency>

    <!-- Struts 2 JSON Plugins -->
    <dependency>
          <groupId>org.apache.struts</groupId>
	  <artifactId>struts2-json-plugin</artifactId>
	  <version>2.1.8</version>
    </dependency> 
```

 ## 2.行动(JSON)

这是一个将被转换成 JSON 格式的动作类。

```java
 package com.mkyong.common.action;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import com.opensymphony.xwork2.Action;

public class JSONDataAction{

	private String string1 = "A";
	private String[] stringarray1 = {"A1","B1"};
	private int number1 = 123456789;
	private int[] numberarray1 = {1,2,3,4,5,6,7,8,9};
	private List<String> lists = new ArrayList<String>();
	private Map<String, String> maps = new HashMap<String, String>();

	//no getter method, will not include in the JSON
	private String string2 = "B";

	public JSONDataAction(){
		lists.add("list1");
		lists.add("list2");
		lists.add("list3");
		lists.add("list4");
		lists.add("list5");

		maps.put("key1", "value1");
		maps.put("key2", "value2");
		maps.put("key3", "value3");
		maps.put("key4", "value4");
		maps.put("key5", "value5");
	}

	public String execute() {
               return Action.SUCCESS;
        }

	public String getString1() {
		return string1;
	}

	public void setString1(String string1) {
		this.string1 = string1;
	}

	public String[] getStringarray1() {
		return stringarray1;
	}

	public void setStringarray1(String[] stringarray1) {
		this.stringarray1 = stringarray1;
	}

	public int getNumber1() {
		return number1;
	}

	public void setNumber1(int number1) {
		this.number1 = number1;
	}

	public int[] getNumberarray1() {
		return numberarray1;
	}

	public void setNumberarray1(int[] numberarray1) {
		this.numberarray1 = numberarray1;
	}

	public List<String> getLists() {
		return lists;
	}

	public void setLists(List<String> lists) {
		this.lists = lists;
	}

	public Map<String, String> getMaps() {
		return maps;
	}

	public void setMaps(Map<String, String> maps) {
		this.maps = maps;
	}

} 
```

 ## 3.struts.xml

要输出 JSON 数据，需要声明一个扩展了“ **json-default** ”的包，结果类型为“ **json** ”。

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>

   <constant name="struts.devMode" value="true" />

   <package name="default" namespace="/" extends="json-default">
      <action name="getJSONResult" 
           class="com.mkyong.common.action.JSONDataAction">
       	   <result type="json" />
      </action>
    </package>

</struts> 
```

## 4.演示

访问动作 URL， **JSONDataAction 的属性**将被转换成 JSON 格式。
*http://localhost:8080/struts 2 example/getjsonresult . action*

![Struts 2 JSON example](img/9e1ef6be5d0cfa6e26f48c6859094d54.png "struts2-json-example")

JSON 格式…

```java
{
   "lists":["list1","list2","list3","list4","list5"],
   "maps":
   {
     "key4":"value4","key3":"value3","key5":"value5","key2":"value2","key1":"value1"
   },
   "number1":123456789,
   "numberarray1":[1,2,3,4,5,6,7,8,9],
   "string1":"A",
   "stringarray1":["A1","B1"]
}

```

Hope this super simple example can give you an overall idea of how JSON plugin worked with Struts 2\. However, there are still many useful settings are not cover here, make sure you read the [Struts 2 JSON plugin](http://web.archive.org/web/20190304030439/http://struts.apache.org/2.1.8.1/docs/json-plugin.html) documentation for more details.

## 下载源代码

Download It – [Struts2-JSON-Example.zip](http://web.archive.org/web/20190304030439/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-JSON-Example.zip)

## 参考

1.  [Struts 2 JSON 插件](http://web.archive.org/web/20190304030439/http://struts.apache.org/2.1.8.1/docs/json-plugin.html)
2.  [JSON 官方文档](http://web.archive.org/web/20190304030439/http://www.json.org/)

[json](http://web.archive.org/web/20190304030439/http://www.mkyong.com/tag/json/) [struts2](http://web.archive.org/web/20190304030439/http://www.mkyong.com/tag/struts2/)







