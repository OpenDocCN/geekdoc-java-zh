# Spring EL 列表，地图示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-el-lists-maps-example/>

在本文中，我们将向您展示如何使用 Spring EL 从**映射**和**列表**中获取值。实际上，SpEL 处理 Map 和 List 的方式和 Java 完全一样。参见示例:

```java
 //get map whete key = 'MapA'
	@Value("#{testBean.map['MapA']}")
	private String mapA;

	//get first value from list, list is 0-based.
	@Value("#{testBean.list[0]}")
	private String list; 
```

## 注释中的弹簧 EL

这里，创建了一个`HashMap`和`ArrayList`，用一些初始数据进行测试。

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("customerBean")
public class Customer {

	@Value("#{testBean.map['MapA']}")
	private String mapA;

	@Value("#{testBean.list[0]}")
	private String list;

	public String getMapA() {
		return mapA;
	}

	public void setMapA(String mapA) {
		this.mapA = mapA;
	}

	public String getList() {
		return list;
	}

	public void setList(String list) {
		this.list = list;
	}

	@Override
	public String toString() {
		return "Customer [mapA=" + mapA + ", list=" + list + "]";
	}

} 
```

```java
 package com.mkyong.core;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import org.springframework.stereotype.Component;

@Component("testBean")
public class Test {

	private Map<String, String> map;
	private List<String> list;

	public Test() {
		map = new HashMap<String, String>();
		map.put("MapA", "This is A");
		map.put("MapB", "This is B");
		map.put("MapC", "This is C");

		list = new ArrayList<String>();
		list.add("List0");
		list.add("List1");
		list.add("List2");

	}

	public Map<String, String> getMap() {
		return map;
	}

	public void setMap(Map<String, String> map) {
		this.map = map;
	}

	public List<String> getList() {
		return list;
	}

	public void setList(List<String> list) {
		this.list = list;
	}

} 
```

*运行它*

```java
 Customer obj = (Customer) context.getBean("customerBean");
       System.out.println(obj); 
```

*输出*

```java
 Customer [mapA=This is A, list=List0] 
```

 ## XML 中的 Spring EL

请参见 bean 定义 XML 文件中的等效版本。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="customerBean" class="com.mkyong.core.Customer">
		<property name="mapA" value="#{testBean.map['MapA']}" />
		<property name="list" value="#{testBean.list[0]}" />
	</bean>

	<bean id="testBean" class="com.mkyong.core.Test" />

</beans> 
```

 ## 下载源代码

Download It – [Spring3-EL-Map-List-Example.zip](http://web.archive.org/web/20190309055305/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Map-List-Example.zip) (6 KB)[list](http://web.archive.org/web/20190309055305/http://www.mkyong.com/tag/list/) [map](http://web.archive.org/web/20190309055305/http://www.mkyong.com/tag/map/) [spring el](http://web.archive.org/web/20190309055305/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190309055305/http://www.mkyong.com/tag/spring3/)







