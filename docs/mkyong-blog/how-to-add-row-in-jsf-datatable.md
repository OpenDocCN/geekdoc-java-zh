# 如何在 JSF 数据表中添加行

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-add-row-in-jsf-datatable/>

这个例子增强了前面的[删除数据表行的例子](http://web.archive.org/web/20210122095705/http://www.mkyong.com/jsf2/how-to-delete-row-in-jsf-datatable/)，通过添加一个“add”函数在数据表中添加一行。

下面是一个 JSF 2.0 的例子，向您展示如何在数据表中添加一行。

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

	String orderNo;
	String productName;
	BigDecimal price;
	int qty;

	//getter and setter methods

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

	public String addAction() {

		Order order = new Order(this.orderNo, this.productName, 
			this.price, this.qty);

		orderList.add(order);
		return null;
	}

	public String deleteAction(Order order) {

		orderList.remove(order);
		return null;
	}

	public static class Order{

		String orderNo;
		String productName;
		BigDecimal price;
		int qty;

		public Order(String orderNo, String productName, 
				BigDecimal price, int qty) {
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

显示带有 dataTable 标记的数据的 JSF 页面，以及键入订单数据的条目表单。

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
    			#{o.orderNo}

    		</h:column>

    		<h:column>

    			<f:facet name="header">Product Name</f:facet>
    			#{o.productName}

    		</h:column>

    		<h:column>

    			<f:facet name="header">Price</f:facet>
    			#{o.price}

    		</h:column>

    		<h:column>

    			<f:facet name="header">Quantity</f:facet>
    			#{o.qty}

    		</h:column>

    		<h:column>

    			<f:facet name="header">Action</f:facet>

    			<h:commandLink value="Delete" 
                                   action="#{order.deleteAction(o)}" />

    		</h:column>

    		</h:dataTable>

    		<h2>Enter Order</h2>
    		<table>
    		<tr>
    			<td>Order No :</td>
    			<td><h:inputText size="10" value="#{order.orderNo}" /></td>
    		</tr>
    		<tr>
    			<td>Product Name :</td>
    			<td><h:inputText size="20" value="#{order.productName}" /></td>
    		</tr>
    		<tr>
    			<td>Quantity :</td>
    			<td><h:inputText size="5" value="#{order.price}" /></td>
    		</tr>
    		<tr>
    			<td>Price :</td>
    			<td><h:inputText size="10" value="#{order.qty}" /></td>
    		</tr>
    		</table>

    		<h:commandButton value="Add" action="#{order.addAction}" />

    	</h:form>
    </h:body>
</html> 
```

## 3.演示

从上到下，显示正在添加的行记录。



![jsf2-dataTable-Add-Example-1](img/c4e835131adb57d10ea3d809541b8200.png "jsf2-dataTable-Add-Example-1")



![jsf2-dataTable-Add-Example-2](img/a979a6d433d36fd11c0b06b49ad296a6.png "jsf2-dataTable-Add-Example-2")



![jsf2-dataTable-Add-Example-3](img/e68aa15fe71f4fed5cb471f68281e673.png "jsf2-dataTable-Add-Example-3")

## 下载源代码

Download It – [JSF-2-DataTable-Add-Example.zip](http://web.archive.org/web/20210122095705/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-DataTable-Add-Example.zip) (10KB)Tags : [datatable](http://web.archive.org/web/20210122095705/https://mkyong.com/tag/datatable/) [insert](http://web.archive.org/web/20210122095705/https://mkyong.com/tag/insert/) [jsf2](http://web.archive.org/web/20210122095705/https://mkyong.com/tag/jsf2/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="7405">

### 相关文章

*   [JSF 2 数据表示例](/web/20210122095705/https://mkyong.com/jsf2/jsf-2-datatable-example/)
*   [如何更新 JSF 数据表中的行](/web/20210122095705/https://mkyong.com/jsf2/how-to-update-row-in-jsf-datatable/)
*   [如何删除 JSF 数据表中的行](/web/20210122095705/https://mkyong.com/jsf2/how-to-delete-row-in-jsf-datatable/)
*   [如何在 JSF 显示数据表行号](/web/20210122095705/https://mkyong.com/jsf2/how-to-display-datatable-row-numbers-in-jsf/)
*   [JSF 2 数据表排序示例](/web/20210122095705/https://mkyong.com/jsf2/jsf-2-datatable-sorting-example/)

*   [JSF 2 数据表排序示例-数据模型](/web/20210122095705/https://mkyong.com/jsf2/jsf-2-datatable-sorting-example-datamodel/)
*   [休眠-如果列名为 ke](/web/20210122095705/https://mkyong.com/hibernate/hibernate-unable-to-insert-if-column-named-is-keyword-such-as-desc/) 则无法插入
*   [JSF 2.0 教程](/web/20210122095705/https://mkyong.com/tutorials/jsf-2-0-tutorials/)
*   [JSF 2 预渲染事件示例](/web/20210122095705/https://mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)
*   [JSF 2.0 中的多组件验证器](/web/20210122095705/https://mkyong.com/jsf2/multi-components-validator-in-jsf-2-0/)