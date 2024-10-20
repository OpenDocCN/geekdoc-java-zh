# 如何更新 JSF 数据表中的行

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-update-row-in-jsf-datatable/>

这个例子增强了前面的 [JSF 2 数据表例子](http://web.archive.org/web/20190210095239/http://www.mkyong.com/jsf2/jsf-2-datatable-example/)，通过添加一个**更新**函数来更新数据表中的行。

## 更新概念

整体概念非常简单:

1.添加一个“ediatble”属性来跟踪行编辑状态。

```java
 //...
public class Order{

	String orderNo;
	String productName;
	BigDecimal price;
	int qty;

	boolean editable;

	public boolean isEditable() {
		return editable;
	}
	public void setEditable(boolean editable) {
		this.editable = editable;
	} 
```

2.在每一行的末尾分配一个“编辑”链接，如果点击，设置“ediatble”= true。在 JSF 2.0 中，您可以直接在方法表达式中提供参数值，请参见下面的编辑操作:

```java
 //...
<h:dataTable value="#{order.orderList}" var="o">

<h:column>

    	<f:facet name="header">Action</f:facet>

    	<h:commandLink value="Edit" action="#{order.editAction(o)}" rendered="#{not o.editable}" />

</h:column> 
```

```java
 //...
public String editAction(Order order) {

	order.setEditable(true);
	return null;
} 
```

3.在 JSF 页面中，如果“edia tble”= true，显示输入文本框进行编辑；否则，只显示正常的输出文本。一个简单的模拟更新效果的小技巧:)

```java
 //...
<h:dataTable value="#{order.orderList}" var="o">

<h:column>

    <f:facet name="header">Order No</f:facet>

    <h:inputText value="#{o.orderNo}" size="10" rendered="#{o.editable}" />

    <h:outputText value="#{o.orderNo}" rendered="#{not o.editable}" />

</h:column> 
```

4.最后，提供一个按钮来保存您的更改。当您在输入文本框中进行更改并保存时，所有值将自动绑定到相关的支持 bean。

```java
 //...
<h:commandButton value="Save Changes" action="#{order.saveAction}" />
</h:column> 
```

 ## 例子

一个 JSF 2.0 的例子来实现上述概念，以更新数据表中的行。

 ## 1.受管 Bean

一个名为“order”的托管 bean，不言自明。

```java
 package com.mkyong;

import java.io.Serializable;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Arrays;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="order")
@SessionScoped
public class OrderBean implements Serializable{

	private static final long serialVersionUID = 1L;

	private static final ArrayList<Order> orderList = 
		new ArrayList<Order>(Arrays.asList(

		new Order("A0001", "Intel CPU", 
				new BigDecimal("700.00"), 1),
		new Order("A0002", "Harddisk 10TB", 
				new BigDecimal("500.00"), 2),
		new Order("A0003", "Dell Laptop", 
				new BigDecimal("11600.00"), 8),
		new Order("A0004", "Samsung LCD", 
				new BigDecimal("5200.00"), 3),
		new Order("A0005", "A4Tech Mouse", 
				new BigDecimal("100.00"), 10)
	));

	public ArrayList<Order> getOrderList() {
		return orderList;
	}

	public String saveAction() {

		//get all existing value but set "editable" to false 
		for (Order order : orderList){
			order.setEditable(false);
		}
		//return to current page
		return null;

	}

	public String editAction(Order order) {

		order.setEditable(true);
		return null;
	}

	public static class Order{

		String orderNo;
		String productName;
		BigDecimal price;
		int qty;
		boolean editable;

		public Order(String orderNo, String productName, BigDecimal price, int qty) {
			this.orderNo = orderNo;
			this.productName = productName;
			this.price = price;
			this.qty = qty;
		}

		//getter and setter methods
	}
} 
```

## 2.JSF·佩奇

JSF 页面显示带有 dataTable 标记的数据，并创建一个“编辑”链接来更新行记录。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      >
    <h:head>
    	<h:outputStylesheet library="css" name="table-style.css"  />
    </h:head>
    <h:body>

    	<h1>JSF 2 dataTable example</h1>
    	<h:form>
    	   <h:dataTable value="#{order.orderList}" var="o"
    		styleClass="order-table"
    		headerClass="order-table-header"
    		rowClasses="order-table-odd-row,order-table-even-row"
    	   >

    	     <h:column>

                <f:facet name="header">Order No</f:facet>

    		<h:inputText value="#{o.orderNo}" size="10" rendered="#{o.editable}" />

    		<h:outputText value="#{o.orderNo}" rendered="#{not o.editable}" />

    	     </h:column>

    	     <h:column>

    		<f:facet name="header">Product Name</f:facet>

    		<h:inputText value="#{o.productName}" size="20" rendered="#{o.editable}" />

    		<h:outputText value="#{o.productName}" rendered="#{not o.editable}" />

    	     </h:column>

    	     <h:column>

    		<f:facet name="header">Price</f:facet>

    		<h:inputText value="#{o.price}" size="10" rendered="#{o.editable}" />

    		<h:outputText value="#{o.price}" rendered="#{not o.editable}" />

    	     </h:column>

    	     <h:column>

    		<f:facet name="header">Quantity</f:facet>

    		<h:inputText value="#{o.qty}" size="5" rendered="#{o.editable}" />

    		<h:outputText value="#{o.qty}" rendered="#{not o.editable}" />

    	     </h:column>

    	     <h:column>

    		<f:facet name="header">Action</f:facet>

    		<h:commandLink value="Edit" action="#{order.editAction(o)}" 
                                       rendered="#{not o.editable}" />

    	     </h:column>

    	  </h:dataTable>

    	  <h:commandButton value="Save Changes" action="#{order.saveAction}" />

      </h:form>
    </h:body>	
</html> 
```

## 3.演示

从上到下，显示正在更新的行记录。

![jsf2-dataTable-Update-Example-1](img/d7c06afda21c8aad6c2085be4c2b3b1e.png "jsf2-dataTable-Update-Example-1")![jsf2-dataTable-Update-Example-2](img/4e2442fabc08915571cfd47127e52226.png "jsf2-dataTable-Update-Example-2")![jsf2-dataTable-Update-Example-3](img/26199732ce0eb01b44fefe7bbdb2e7ad.png "jsf2-dataTable-Update-Example-3")![jsf2-dataTable-Update-Example-4](img/673933193bb1b81562c33edff93b287e.png "jsf2-dataTable-Update-Example-4")

## 下载源代码

Download It – [JSF-2-DataTable-Update-Example.zip](http://web.archive.org/web/20190210095239/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-DataTable-Update-Example.zip) (10KB)[datatable](http://web.archive.org/web/20190210095239/http://www.mkyong.com/tag/datatable/) [jsf2](http://web.archive.org/web/20190210095239/http://www.mkyong.com/tag/jsf2/) [update](http://web.archive.org/web/20190210095239/http://www.mkyong.com/tag/update/)







