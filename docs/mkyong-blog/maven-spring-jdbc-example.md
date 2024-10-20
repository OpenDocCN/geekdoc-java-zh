# 春天+ JDBC 的例子

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/maven-spring-jdbc-example/>

在本教程中，我们将通过添加 JDBC 支持来扩展上一个 [Maven + Spring hello world 示例](http://web.archive.org/web/20221225035543/http://www.mkyong.com/spring/quick-start-maven-spring-example/)，使用 Spring + JDBC 将记录插入到客户表中。

## 1.客户表

在这个例子中，我们使用 MySQL 数据库。

```java
 CREATE TABLE `customer` (
  `CUST_ID` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `NAME` varchar(100) NOT NULL,
  `AGE` int(10) unsigned NOT NULL,
  PRIMARY KEY (`CUST_ID`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8; 
```

## 2.项目依赖性

在 Maven `pom.xml`文件中添加 Spring 和 MySQL 依赖项。

*文件:pom.xml*

```java
 <project  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
  http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mkyong.common</groupId>
  <artifactId>SpringExample</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>SpringExample</name>
  <url>http://maven.apache.org</url>

  <dependencies>

        <!-- Spring framework -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring</artifactId>
		<version>2.5.6</version>
	</dependency>

        <!-- MySQL database driver -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.9</version>
	</dependency>

  </dependencies>
</project> 
```

## 3.客户模型

添加客户模型以存储客户数据。

```java
 package com.mkyong.customer.model;

import java.sql.Timestamp;

public class Customer 
{
	int custId;
	String name;
	int age;
	//getter and setter methods

} 
```

## 4.数据访问对象(DAO)模式

客户 Dao 接口。

```java
 package com.mkyong.customer.dao;

import com.mkyong.customer.model.Customer;

public interface CustomerDAO 
{
	public void insert(Customer customer);
	public Customer findByCustomerId(int custId);
} 
```

客户 Dao 实现，使用 JDBC 发出简单的 insert 和 select 语句。

```java
 package com.mkyong.customer.dao.impl;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import javax.sql.DataSource;
import com.mkyong.customer.dao.CustomerDAO;
import com.mkyong.customer.model.Customer;

public class JdbcCustomerDAO implements CustomerDAO
{
	private DataSource dataSource;

	public void setDataSource(DataSource dataSource) {
		this.dataSource = dataSource;
	}

	public void insert(Customer customer){

		String sql = "INSERT INTO CUSTOMER " +
				"(CUST_ID, NAME, AGE) VALUES (?, ?, ?)";
		Connection conn = null;

		try {
			conn = dataSource.getConnection();
			PreparedStatement ps = conn.prepareStatement(sql);
			ps.setInt(1, customer.getCustId());
			ps.setString(2, customer.getName());
			ps.setInt(3, customer.getAge());
			ps.executeUpdate();
			ps.close();

		} catch (SQLException e) {
			throw new RuntimeException(e);

		} finally {
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {}
			}
		}
	}

	public Customer findByCustomerId(int custId){

		String sql = "SELECT * FROM CUSTOMER WHERE CUST_ID = ?";

		Connection conn = null;

		try {
			conn = dataSource.getConnection();
			PreparedStatement ps = conn.prepareStatement(sql);
			ps.setInt(1, custId);
			Customer customer = null;
			ResultSet rs = ps.executeQuery();
			if (rs.next()) {
				customer = new Customer(
					rs.getInt("CUST_ID"),
					rs.getString("NAME"), 
					rs.getInt("Age")
				);
			}
			rs.close();
			ps.close();
			return customer;
		} catch (SQLException e) {
			throw new RuntimeException(e);
		} finally {
			if (conn != null) {
				try {
				conn.close();
				} catch (SQLException e) {}
			}
		}
	}
} 
```

## 5.弹簧豆配置

为 customerDAO 和 datasource 创建 Spring bean 配置文件。
*文件:Spring-Customer.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerDAO" class="com.mkyong.customer.dao.impl.JdbcCustomerDAO">
		<property name="dataSource" ref="dataSource" />
	</bean>

</beans> 
```

*文件:Spring-Datasource.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">

		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/mkyongjava" />
		<property name="username" value="root" />
		<property name="password" value="password" />
	</bean>

</beans> 
```

*文件:Spring-Module.xml*

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<import resource="database/Spring-Datasource.xml" />
	<import resource="customer/Spring-Customer.xml" />

</beans> 
```

## 6.检查项目结构

这个例子的完整目录结构。

![](img/979ad51612c219e4310eae11d3059f0d.png "maven-spring-jdbc")

## 7.运行它

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.mkyong.customer.dao.CustomerDAO;
import com.mkyong.customer.model.Customer;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
    		new ClassPathXmlApplicationContext("Spring-Module.xml");

        CustomerDAO customerDAO = (CustomerDAO) context.getBean("customerDAO");
        Customer customer = new Customer(1, "mkyong",28);
        customerDAO.insert(customer);

        Customer customer1 = customerDAO.findByCustomerId(1);
        System.out.println(customer1);

    }
} 
```

输出

```java
 Customer [age=28, custId=1, name=mkyong] 
```

## 下载源代码

Download it– [SpringJDBCExample.zip](http://web.archive.org/web/20221225035543/http://www.mkyong.com/wp-content/uploads/2010/03/SpringJDBCExample.zip) (10 KB)<input type="hidden" id="mkyong-current-postId" value="3763">