# JSF 2 数据表排序示例–数据模型

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-datatable-sorting-example-datamodel/>

在前面的 [JSF 2 的数据表排序示例](http://web.archive.org/web/20190214232804/http://www.mkyong.com/jsf2/jsf-2-datatable-sorting-example/)中，展示了最简单的方法，一个自定义的比较器来排序一个列表并显示在数据表中。

## 数据模型装饰器

在这个例子中，展示了另一种对 dataTable 中的列表进行排序的方式，在“ *Core JavaServer Faces(第三版)*”中提到了这种方式，称为 DataModel Decorator。

 ## 1.数据模型

创建一个 decorator 类来扩展 javax.faces.model.DataModel 类，并添加一个额外的排序行为。嗯，解释起来有点复杂，详细请参考书中:)

```java
 package com.mkyong;

import java.util.Arrays;
import java.util.Comparator;
import javax.faces.model.DataModel;

public class SortableDataModel<E> extends DataModel<E>{

	DataModel<E> model;
	private Integer[] rows;

	SortableDataModel(DataModel<E> model){
		this.model = model;
		initRows();
	}

	public void initRows(){
		int rowCount = model.getRowCount();
		if(rowCount != -1){
			this.rows = new Integer[rowCount];
			for(int i = 0; i < rowCount; ++i){
				rows[i] = i;
			}
		}
	}

	public void sortBy(final Comparator<E> comparator){
		Comparator<Integer> rowComp = new Comparator<Integer>() {
			public int compare(Integer i1, Integer i2){
				E o1 = getData(i1);
				E o2 = getData(i2);
				return comparator.compare(o1, o2);
			}
		};
		Arrays.sort(rows, rowComp);

	}

	private E getData(int row){
		int originalRowIndex = model.getRowIndex();

		model.setRowIndex(row);
		E newRowData = model.getRowData();
		model.setRowIndex(originalRowIndex);

		return newRowData;
	}

	@Override
	public void setRowIndex(int rowIndex) {

		if(0 <= rowIndex && rowIndex < rows.length){
			model.setRowIndex(rows[rowIndex]);
		}else{
			model.setRowIndex(rowIndex);
		}
	}

	@Override
	public boolean isRowAvailable() {
		return model.isRowAvailable();
	}

	@Override
	public int getRowCount() {
		return model.getRowCount();
	}

	@Override
	public E getRowData() {
		return model.getRowData();
	}

	@Override
	public int getRowIndex() {
		return model.getRowIndex();
	}

	@Override
	public Object getWrappedData() {
		return model.getWrappedData();
	}

	@Override
	public void setWrappedData(Object data) {

		model.setWrappedData(data);
		initRows();

	}

} 
```

 ## 2.受管 Bean

一个托管 bean 为测试提供一个虚拟列表，并展示了使用**自定义数据模型对数据表列表**进行排序。

```java
 package com.mkyong;

import java.io.Serializable;
import java.math.BigDecimal;
import java.util.Comparator;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.faces.model.ArrayDataModel;
import javax.faces.model.DataModel;

@ManagedBean(name="order")
@SessionScoped
public class OrderBean implements Serializable{

	private static final long serialVersionUID = 1L;

	private SortableDataModel<Order> sotableDataModel;

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

		sotableDataModel = new SortableDataModel<Order>(
			new ArrayDataModel<Order>(orderList));

	}

	public DataModel<Order> getOrderList() {

		return sotableDataModel;

	}

	//sort by order no
	public String sortByOrderNo() {

		if(sortAscending){

		  sotableDataModel.sortBy(new Comparator<Order>() {

			@Override
			public int compare(Order o1, Order o2) {

				return o1.getOrderNo().compareTo(o2.getOrderNo());

			}
		});

		sortAscending = false;

	  }else{

		//descending order
		sotableDataModel.sortBy(new Comparator<Order>() {

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

		//getter and setter methods

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

![jsf2-dataTable-Sorting-Example-1](img/7dba250e08d53da11444a64563963eda.png "jsf2-dataTable-Sorting-Example-1")![jsf2-dataTable-Sorting-Example-2](img/631e0020fbdd203cafe84a9e747977a9.png "jsf2-dataTable-Sorting-Example-2")![jsf2-dataTable-Sorting-Example-3](img/ccda8a792aaa6455f9d5d44a4a3606eb.png "jsf2-dataTable-Sorting-Example-3")

## 下载源代码

Download It - [JSF-2-DataTable-Sorting-DataModel-Example.zip](http://web.archive.org/web/20190214232804/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-DataTable-Sorting-DataModel-Example.zip) (11KB)

## 参考

1.  [JSF 2 数据表排序示例](http://web.archive.org/web/20190214232804/http://www.mkyong.com/jsf2/jsf-2-datatable-sorting-example/)
2.  [Java 对象排序示例](http://web.archive.org/web/20190214232804/http://www.mkyong.com/java/java-object-sorting-example-comparable-and-comparator/)

[datatable](http://web.archive.org/web/20190214232804/http://www.mkyong.com/tag/datatable/) [jsf2](http://web.archive.org/web/20190214232804/http://www.mkyong.com/tag/jsf2/) [sorting](http://web.archive.org/web/20190214232804/http://www.mkyong.com/tag/sorting/)







