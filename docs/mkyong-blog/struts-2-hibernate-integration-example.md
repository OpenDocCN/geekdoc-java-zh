# Struts 2 + Hibernate 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-hibernate-integration-example/>

Download it – [Struts2-Hibernate-Integration-Example.zip](http://web.archive.org/web/20190306164739/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-Hibernate-Integration-Example.zip)

在 Struts2 中，没有集成 Hibernate 框架的官方插件。但是，您可以通过以下步骤解决这个问题:

1.  注册一个自定义的 **ServletContextListener** 。
2.  在 **ServletContextListener** 类中，初始化 **Hibernate 会话，并将其存储到 servlet 上下文**中。
3.  在 action 类中，**从 servlet 上下文**中获取 Hibernate 会话，并正常执行 Hibernate 任务。

查看关系:

```java
 Struts 2 <-- (Servlet Context) ---> Hibernate <-----> Database 
```

在本教程中，展示了一个简单的客户模块(添加和列表功能)，用 **Struts 2** 开发，用 **Hibernate** 执行数据库操作。集成部分使用上述机制(在 servlet 上下文中存储和检索 Hibernate 会话)。

This workaround is very similar with the classic [Struts 1.x and Hibernate integration](http://web.archive.org/web/20190306164739/http://www.mkyong.com/struts/struts-hibernate-integration-example/), just the classic Struts 1.x is using the Struts’s plugins; While the Struts 2 is using the generic servlet listener.

## 1.项目结构

查看完整的项目文件夹结构。

![Struts 2 Hibernate integration example](img/fda85e00473c2af9fc40a8966e619caf.png "Struts2-Hibernate-Integration-Example") ## 2.MySQL 表脚本

为演示创建一个客户表。下面是 SQL 表脚本。

```java
 DROP TABLE IF EXISTS `mkyong`.`customer`;
CREATE TABLE  `mkyong`.`customer` (
  `CUSTOMER_ID` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `NAME` varchar(45) NOT NULL,
  `ADDRESS` varchar(255) NOT NULL,
  `CREATED_DATE` datetime NOT NULL,
  PRIMARY KEY (`CUSTOMER_ID`)
) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=utf8; 
```

 ## 3.属国

获取 Struts2、Hibernate 和 MySQL 依赖库。

```java
 //...
	<!-- Struts 2 -->
	<dependency>
	        <groupId>org.apache.struts</groupId>
		<artifactId>struts2-core</artifactId>
		<version>2.1.8</version>
        </dependency>

	<!-- MySQL database driver -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.9</version>
	</dependency>

	<!-- Hibernate core -->
	<dependency>
		<groupId>org.hibernate</groupId>
		<artifactId>hibernate</artifactId>
		<version>3.2.7.ga</version>
	</dependency>

	<!-- Hibernate core library dependency start -->
	<dependency>
		<groupId>dom4j</groupId>
		<artifactId>dom4j</artifactId>
		<version>1.6.1</version>
	</dependency>

	<dependency>
		<groupId>commons-logging</groupId>
		<artifactId>commons-logging</artifactId>
		<version>1.1.1</version>
	</dependency>

	<dependency>
		<groupId>commons-collections</groupId>
		<artifactId>commons-collections</artifactId>
		<version>3.2.1</version>
	</dependency>

	<dependency>
		<groupId>cglib</groupId>
		<artifactId>cglib</artifactId>
		<version>2.2</version>
	</dependency>
	<!-- Hibernate core library dependency end -->

	<!-- Hibernate query library dependency start -->
	<dependency>
		<groupId>antlr</groupId>
		<artifactId>antlr</artifactId>
		<version>2.7.7</version>
	</dependency>
	<!-- Hibernate query library dependency end -->
//... 
```

## 4.冬眠的东西

Hibernate 模型和配置材料。

**Customer.java**–为客户表创建一个类。

```java
 package com.mkyong.customer.model;

import java.util.Date;

public class Customer implements java.io.Serializable {

	private Long customerId;
	private String name;
	private String address;
	private Date createdDate;

	//getter and setter methods
} 
```

**customer . hbm . XML**–客户的 Hibernate 映射文件。

```java
 <?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="com.mkyong.customer.model.Customer" 
	table="customer" catalog="mkyong">

        <id name="customerId" type="java.lang.Long">
            <column name="CUSTOMER_ID" />
            <generator class="identity" />
        </id>
        <property name="name" type="string">
            <column name="NAME" length="45" not-null="true" />
        </property>
        <property name="address" type="string">
            <column name="ADDRESS" not-null="true" />
        </property>
        <property name="createdDate" type="timestamp">
            <column name="CREATED_DATE" length="19" not-null="true" />
        </property>
    </class>
</hibernate-mapping> 
```

**Hibernate . CFG . XML**–Hibernate 数据库配置文件。

```java
 <?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
  <session-factory>
    <property name="hibernate.bytecode.use_reflection_optimizer">false</property>
    <property name="hibernate.connection.password">password</property>
    <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mkyong</property>
    <property name="hibernate.connection.username">root</property>
    <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
    <property name="show_sql">true</property>
    <property name="format_sql">true</property>
    <property name="use_sql_comments">false</property>
    <mapping resource="com/mkyong/customer/hibernate/Customer.hbm.xml" />
  </session-factory>
</hibernate-configuration> 
```

## 5.hibernate 上下文侦听器

创建一个 **ServletContextListener** ，初始化 **Hibernate 会话，并将其存储到 servlet context** 中。

**冬眠监听器。java**

```java
 package com.mkyong.listener;

import java.net.URL;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateListener implements ServletContextListener{

    private Configuration config;
    private SessionFactory factory;
    private String path = "/hibernate.cfg.xml";
    private static Class clazz = HibernateListener.class;

    public static final String KEY_NAME = clazz.getName();

	public void contextDestroyed(ServletContextEvent event) {
	  //
	}

	public void contextInitialized(ServletContextEvent event) {

	 try { 
	        URL url = HibernateListener.class.getResource(path);
	        config = new Configuration().configure(url);
	        factory = config.buildSessionFactory();

	        //save the Hibernate session factory into serlvet context
	        event.getServletContext().setAttribute(KEY_NAME, factory);
	  } catch (Exception e) {
	         System.out.println(e.getMessage());
	   }
	}
} 
```

在 web.xml 文件中注册侦听器。
**web.xml**

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Struts 2 Web Application</display-name>

  <filter>
	<filter-name>struts2</filter-name>
	<filter-class>
	  org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
	</filter-class>
  </filter>

  <filter-mapping>
	<filter-name>struts2</filter-name>
	<url-pattern>/*</url-pattern>
  </filter-mapping>

  <listener>
    <listener-class>
	  com.mkyong.listener.HibernateListener
    </listener-class>
  </listener>

</web-app> 
```

## 6.行动

在 Action 类中，**从 servlet 上下文**中获取 Hibernate 会话，并正常执行 Hibernate 任务。

**CustomerAction.java**

```java
 package com.mkyong.customer.action;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.apache.struts2.ServletActionContext;
import org.hibernate.Session;
import org.hibernate.SessionFactory;

import com.mkyong.customer.model.Customer;
import com.mkyong.listener.HibernateListener;
import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;

public class CustomerAction extends ActionSupport 
	implements ModelDriven{

	Customer customer = new Customer();
	List<Customer> customerList = new ArrayList<Customer>();

	public String execute() throws Exception {
		return SUCCESS;
	}

	public Object getModel() {
		return customer;
	}

	public List<Customer> getCustomerList() {
		return customerList;
	}

	public void setCustomerList(List<Customer> customerList) {
		this.customerList = customerList;
	}

	//save customer
	public String addCustomer() throws Exception{

		//get hibernate session from the servlet context
		SessionFactory sessionFactory = 
	         (SessionFactory) ServletActionContext.getServletContext()
                     .getAttribute(HibernateListener.KEY_NAME);

		Session session = sessionFactory.openSession();

		//save it
		customer.setCreatedDate(new Date());

		session.beginTransaction();
		session.save(customer);
		session.getTransaction().commit();

		//reload the customer list
		customerList = null;
		customerList = session.createQuery("from Customer").list();

		return SUCCESS;

	}

	//list all customers
	public String listCustomer() throws Exception{

		//get hibernate session from the servlet context
		SessionFactory sessionFactory = 
	         (SessionFactory) ServletActionContext.getServletContext()
                     .getAttribute(HibernateListener.KEY_NAME);

		Session session = sessionFactory.openSession();

		customerList = session.createQuery("from Customer").list();

		return SUCCESS;

	}	
} 
```

## 7.JSP 页面

添加和列出客户的 JSP 页面。

**customer.jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
</head>

<body>
<h1>Struts 2 + Hibernate integration example</h1>

<h2>Add Customer</h2>
<s:form action="addCustomerAction" >
  <s:textfield name="name" label="Name" value="" />
  <s:textarea name="address" label="Address" value="" cols="50" rows="5" />
  <s:submit />
</s:form>

<h2>All Customers</h2>

<s:if test="customerList.size() > 0">
<table border="1px" cellpadding="8px">
	<tr>
		<th>Customer Id</th>
		<th>Name</th>
		<th>Address</th>
		<th>Created Date</th>
	</tr>
	<s:iterator value="customerList" status="userStatus">
		<tr>
			<td><s:property value="customerId" /></td>
			<td><s:property value="name" /></td>
			<td><s:property value="address" /></td>
			<td><s:date name="createdDate" format="dd/MM/yyyy" /></td>
		</tr>
	</s:iterator>
</table>
</s:if>
<br/>
<br/>

</body>
</html> 
```

## 8.struts.xml

全部链接起来~

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
  <constant name="struts.devMode" value="true" />

  <package name="default" namespace="/" extends="struts-default">

    <action name="addCustomerAction" 
	class="com.mkyong.customer.action.CustomerAction" method="addCustomer" >
       <result name="success">pages/customer.jsp</result>
    </action>

    <action name="listCustomerAction" 
	class="com.mkyong.customer.action.CustomerAction" method="listCustomer" >
        <result name="success">pages/customer.jsp</result>
    </action>		

  </package>	
</struts> 
```

## 9.演示

访问客户模块:*http://localhost:8080/struts 2 example/listcustomeraction . action*

![Struts 2 Hibernate Add Customer](img/8bb48916cbf208246bdcde86d291b72b.png "Struts2-Hibernate-AddCustomer")

填写姓名和地址字段，点击提交按钮，插入的客户详细信息将立即列出。

![Struts2 Hibernate List Customer](img/3b58f8dc699059072d89e42da154ee38.png "Struts2-Hibernate-ListCustomer")

## 参考

1.  [Struts 2 + Hibernate 与“全 Hibernate 插件”的集成](http://web.archive.org/web/20190306164739/http://www.mkyong.com/struts2/struts-2-hibernate-integration-with-full-hibernate-plugin/)
2.  [ServletContextListener 文档](http://web.archive.org/web/20190306164739/http://java.sun.com/products/servlet/2.3/javadoc/javax/servlet/ServletContextListener.html)
3.  [Struts + Hibernate 集成示例](http://web.archive.org/web/20190306164739/http://www.mkyong.com/struts/struts-hibernate-integration-example/)

[hibernate](http://web.archive.org/web/20190306164739/http://www.mkyong.com/tag/hibernate/) [integration](http://web.archive.org/web/20190306164739/http://www.mkyong.com/tag/integration/) [struts2](http://web.archive.org/web/20190306164739/http://www.mkyong.com/tag/struts2/)







