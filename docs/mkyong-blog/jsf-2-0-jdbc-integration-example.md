# JSF 2.0 + JDBC 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-0-jdbc-integration-example/>

这里有一个指南，向您展示如何通过 JDBC 集成 JSF 2.0 与数据库。在这个例子中，我们使用 MySQL 数据库和 Tomcat web 容器。

*本例的目录结构*

![](img/f1b559b6cd8ffdcd1c2bbb5b8c3597ea.png "jsf2-jdbc-integration-example-folder")

## 1.表结构

创建一个“*客户*”表，并插入五条虚拟记录。稍后，通过 JSF `h:dataTable`显示。

*SQL 命令*

```java
 DROP TABLE IF EXISTS `mkyongdb`.`customer`;
CREATE TABLE  `mkyongdb`.`customer` (
  `CUSTOMER_ID` bigint(20) UNSIGNED NOT NULL AUTO_INCREMENT,
  `NAME` varchar(45) NOT NULL,
  `ADDRESS` varchar(255) NOT NULL,
  `CREATED_DATE` datetime NOT NULL,
  PRIMARY KEY (`CUSTOMER_ID`)
) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=utf8;

insert into mkyongdb.customer(customer_id, name, address, created_date) 
values(1, 'mkyong1', 'address1', now());
insert into mkyongdb.customer(customer_id, name, address, created_date) 
values(2, 'mkyong2', 'address2', now());
insert into mkyongdb.customer(customer_id, name, address, created_date) 
values(3, 'mkyong3', 'address3', now());
insert into mkyongdb.customer(customer_id, name, address, created_date) 
values(4, 'mkyong4', 'address4', now());
insert into mkyongdb.customer(customer_id, name, address, created_date) 
values(5, 'mkyong5', 'address5', now()); 
```

 ## 2.MySQL 数据源

配置一个名为"**JDBC/mkyondb**"的 MySQL 数据源，遵循本文-[如何在 Tomcat 6 中配置 MySQL 数据源](http://web.archive.org/web/20190310100507/http://www.mkyong.com/tomcat/how-to-configure-mysql-datasource-in-tomcat-6/)

 ## 3.模型类

创建一个“ *Customer* ”模型类来存储表记录。

*文件:Customer.java*

```java
 package com.mkyong.customer.model;

import java.util.Date;

public class Customer{

	public long customerID;
	public String name;
	public String address;
	public Date created_date;

	//getter and setter methods 
} 
```

## 4.JDBC 的例子

一个 JSF 2.0 托管 bean，通过`@Resource`注入 data source "**JDBC/mkyondb**，使用普通的 JDBC API 从数据库中检索所有的客户记录并存储到一个列表中。

*文件:CustomerBean.java*

```java
 package com.mkyong;

import java.io.Serializable;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import javax.annotation.Resource;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;

import com.mkyong.customer.model.Customer;

@ManagedBean(name="customer")
@SessionScoped
public class CustomerBean implements Serializable{

	//resource injection
	@Resource(name="jdbc/mkyongdb")
	private DataSource ds;

	//if resource injection is not support, you still can get it manually.
	/*public CustomerBean(){
		try {
			Context ctx = new InitialContext();
			ds = (DataSource)ctx.lookup("java:comp/env/jdbc/mkyongdb");
		} catch (NamingException e) {
			e.printStackTrace();
		}

	}*/

	//connect to DB and get customer list
	public List<Customer> getCustomerList() throws SQLException{

		if(ds==null)
			throw new SQLException("Can't get data source");

		//get database connection
		Connection con = ds.getConnection();

		if(con==null)
			throw new SQLException("Can't get database connection");

		PreparedStatement ps 
			= con.prepareStatement(
			   "select customer_id, name, address, created_date from customer"); 

		//get customer data from database
		ResultSet result =  ps.executeQuery();

		List<Customer> list = new ArrayList<Customer>();

		while(result.next()){
			Customer cust = new Customer();

			cust.setCustomerID(result.getLong("customer_id"));
			cust.setName(result.getString("name"));
			cust.setAddress(result.getString("address"));
			cust.setCreated_date(result.getDate("created_date"));

			//store all data into a List
			list.add(cust);
		}

		return list;
	}
} 
```

## 5.JSF 页面数据表

一个 JSF 2.0 xhtml 页面，使用`h:dataTable`以表格布局格式显示所有客户记录。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >
    <h:head>
    	<h:outputStylesheet library="css" name="table-style.css"  />
    </h:head>

    <h:body>

    	<h1>JSF 2.0 + JDBC Example</h1>

 		<h:dataTable value="#{customer.getCustomerList()}" var="c"
    			styleClass="order-table"
    			headerClass="order-table-header"
    			rowClasses="order-table-odd-row,order-table-even-row"
    		>

    		<h:column>
    			<f:facet name="header">
    				Customer ID
    			</f:facet>
    				#{c.customerID}
    		</h:column>

    		<h:column>
    			<f:facet name="header">
    				Name
				</f:facet>
    				#{c.name}
    		</h:column>

 			<h:column>
    			<f:facet name="header">
    				Address
				</f:facet>
    				#{c.address}
    		</h:column>

    		<h:column>
    			<f:facet name="header">
    				Created Date
				</f:facet>
    				#{c.created_date}
    		</h:column>

    	</h:dataTable>

    </h:body>

</html> 
```

## 6.演示

运行它，查看输出

![](img/f233ddf843a43ecdfa252ee5616a4ad9.png "jsf2-jdbc-integration-example")

## 下载源代码

Download It – [JSF-2-JDBC-Integration-Example.zip](http://web.archive.org/web/20190310100507/http://www.mkyong.com/wp-content/uploads/2010/12/JSF-2-JDBC-Integration-Example.zip) (12KB)

## 参考

1.  [Java 中的 JDBC 例子](http://web.archive.org/web/20190310100507/http://www.mkyong.com/java/how-to-connect-to-mysql-with-jdbc-driver-java/)
2.  [Tomcat 6 中的 JNDI 数据源示例](http://web.archive.org/web/20190310100507/http://tomcat.apache.org/tomcat-6.0-doc/jndi-datasource-examples-howto.html)
3.  [在 Tomcat 6 中配置 MySQL 数据源](http://web.archive.org/web/20190310100507/http://www.mkyong.com/tomcat/how-to-configure-mysql-datasource-in-tomcat-6/)
4.  [JSF 2 数据表示例](http://web.archive.org/web/20190310100507/http://www.mkyong.com/jsf2/jsf-2-datatable-sorting-example/)

[integration](http://web.archive.org/web/20190310100507/http://www.mkyong.com/tag/integration/) [jdbc](http://web.archive.org/web/20190310100507/http://www.mkyong.com/tag/jdbc/) [jsf2](http://web.archive.org/web/20190310100507/http://www.mkyong.com/tag/jsf2/)







