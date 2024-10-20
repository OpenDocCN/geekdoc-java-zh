# JSF 2 数据表排序示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-datatable-sorting-example/>

对 JSF 数据表列表进行排序的想法如下:

## 1.列标题

在列标题中放置一个 commandLink，如果这个链接被点击，对数据表列表进行排序。

```java
 <h:column>
    <f:facet name="header">
    	<h:commandLink action="#{order.sortByOrderNo}">
    	   Order No
    	</h:commandLink>
    </f:facet>
    #{o.orderNo}
</h:column> 
```

 ## 2.履行

在受管 bean 中，使用 **Collections.sort()** 和自定义比较器对列表进行排序。

```java
 @ManagedBean(name="order")
@SessionScoped
public class OrderBean implements Serializable{

	//sort by order no
	public String sortByOrderNo() {

	   Collections.sort(orderArrayList, new Comparator<Order>() {

		@Override
		public int compare(Order o1, Order o2) {

			return o1.getOrderNo().compareTo(o2.getOrderNo());

		}
	   });	
	}
	//...
} 
```

 ## 数据表排序示例

一个在数据表中实现**排序特性的 JSF 2.0 例子。点击"订单号"列标题使列表按"订单号"升序排序；再次点击，按“订单号”降序排列列表。**

## 1.受管 Bean

一个托管 bean，为测试提供一个虚拟列表，并展示了如何使用 **Collections.sort()对数据表列表**进行排序。

```java
 package com.mkyong;

import java.io.Serializable;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="order")
@SessionScoped
public class OrderBean implements Serializable{

	private static final long serialVersionUID = 1L;

	private List<Order> orderArrayList;

	private boolean sortAscending = true; 

	private static final Order[] orderList = {
		new Order("A0002", "Harddisk 100TB", 
				new BigDecimal("500.00"), 3),
		new Order("A0001", "Intel CPU", 
				new BigDecimal("4200.00"), 6),
		new Order("A0004", "Samsung LCD", 
				new BigDecimal("5200.00"), 10),
		new Order("A0003", "Dell Laptop", 
				new BigDecimal("11600.00"), 9),
		new Order("A0005", "A4Tech Mouse", 
				new BigDecimal("200.00"), 20)
	};

	public OrderBean(){

		orderArrayList = new ArrayList<Order>(Arrays.asList(orderList));

	}

	public List<Order> getOrderList() {

		return orderArrayList;

	}

	//sort by order no
	public String sortByOrderNo() {

	   if(sortAscending){

		//ascending order
		Collections.sort(orderArrayList, new Comparator<Order>() {

			@Override
			public int compare(Order o1, Order o2) {

				return o1.getOrderNo().compareTo(o2.getOrderNo());

			}

		});
		sortAscending = false;

	   }else{

		//descending order
		Collections.sort(orderArrayList, new Comparator<Order>() {

			@Override
			public int compare(Order o1, Order o2) {

				return o2.getOrderNo().compareTo(o1.getOrderNo());

			}

		});
		sortAscending = true;
	   }

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

		public String getOrderNo() {
			return orderNo;
		}

		public void setOrderNo(String orderNo) {
			this.orderNo = orderNo;
		}

		public String getProductName() {
			return productName;
		}

		public void setProductName(String productName) {
			this.productName = productName;
		}

		public BigDecimal getPrice() {
			return price;
		}

		public void setPrice(BigDecimal price) {
			this.price = price;
		}

		public int getQty() {
			return qty;
		}

		public void setQty(int qty) {
			this.qty = qty;
		}
	}
} 
```

## 2.数据表标签

一个 JSF 页面，在“订单号”列标题放一个 commandLink 标签，如果点击，对数据表列表进行排序。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      xmlns:c="http://java.sun.com/jsp/jstl/core"
      >
    <h:head>
    	<h:outputStylesheet library="css" name="table-style.css"  />
    </h:head>
    <h:body>

    	<h1>JSF 2 dataTable sorting example</h1>
		  <h:form>
    		<h:dataTable value="#{order.orderList}" var="o"
    			styleClass="order-table"
    			headerClass="order-table-header"
    			rowClasses="order-table-odd-row,order-table-even-row"
    		>

    		<h:column>
    			<f:facet name="header">
    			   <h:commandLink action="#{order.sortByOrderNo}">
    				Order No
    			   </h:commandLink>
    			</f:facet>
    			#{o.orderNo}
    		</h:column>

    		<h:column>
    			<f:facet name="header">
    				Product Name
			</f:facet>
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

    	    </h:dataTable>
    	  </h:form>
    </h:body>
</html> 
```

## 3.演示

从上到下，显示按升序和降序排序的数据表列表。

![jsf2-dataTable-Sorting-Example-1](img/73864c1c6865ca0f22009c83376e8759.png "jsf2-dataTable-Sorting-Example-1")![jsf2-dataTable-Sorting-Example-2](img/8def765f18d95b22c2557dae5682c0fa.png "jsf2-dataTable-Sorting-Example-2")![jsf2-dataTable-Sorting-Example-3](img/262f466a4fd90b875c3e1be6b979967a.png "jsf2-dataTable-Sorting-Example-3")

## 下载源代码

Download It – [JSF-2-DataTable-Sorting-Example.zip](http://web.archive.org/web/20190210094729/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-DataTable-Sorting-Example.zip) (10KB)

## 参考

1.  [使用数据库对数据表列表进行排序](http://web.archive.org/web/20190210094729/http://balusc.blogspot.com/2006/06/using-datatables.html#SortingDatatable)
2.  JSF h:数据表 JavaDoc
3.  [JSF 2 链接、命令链接和输出链接示例](http://web.archive.org/web/20190210094729/http://www.mkyong.com/jsf2/jsf-2-link-commandlink-and-outputlink-example/)
4.  [Java 对象排序示例](http://web.archive.org/web/20190210094729/http://www.mkyong.com/java/java-object-sorting-example-comparable-and-comparator/)

[datatable](http://web.archive.org/web/20190210094729/http://www.mkyong.com/tag/datatable/) [jsf2](http://web.archive.org/web/20190210094729/http://www.mkyong.com/tag/jsf2/) [sorting](http://web.archive.org/web/20190210094729/http://www.mkyong.com/tag/sorting/)







