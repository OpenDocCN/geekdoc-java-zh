# Spring JdbcTemplate 查询示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring/spring-jdbctemplate-querying-examples/>

这里有几个例子来展示如何使用 JdbcTemplate `query()`方法从数据库中查询或提取数据。

## 1.查询单行

这里有两种方法可以从数据库中查询或提取单行记录，并将其转换为模型类。

 ## 1.1 自定义行映射器

一般来说，总是建议实现 RowMapper 接口来创建自定义的 RowMapper，以满足您的需求。

```java
 package com.mkyong.customer.model;

import java.sql.ResultSet;
import java.sql.SQLException;

import org.springframework.jdbc.core.RowMapper;

public class CustomerRowMapper implements RowMapper
{
	public Object mapRow(ResultSet rs, int rowNum) throws SQLException {
		Customer customer = new Customer();
		customer.setCustId(rs.getInt("CUST_ID"));
		customer.setName(rs.getString("NAME"));
		customer.setAge(rs.getInt("AGE"));
		return customer;
	}

} 
```

将它传递给`queryForObject()`方法，返回的结果将调用您的自定义`mapRow()`方法来正确地将值匹配到。

```java
 public Customer findByCustomerId(int custId){

	String sql = "SELECT * FROM CUSTOMER WHERE CUST_ID = ?";

	Customer customer = (Customer)getJdbcTemplate().queryForObject(
			sql, new Object[] { custId }, new CustomerRowMapper());

	return customer;
} 
```

 ## 1.2 BeanPropertyRowMapper

在 Spring 2.5 中，附带了一个方便的行映射器实现，称为“BeanPropertyRowMapper”，它可以通过匹配它们的名称将行的列值映射到属性。只需确保属性和列具有相同的名称，例如，属性“CUSTID”将与列名“custId”或下划线“CUST_ID”匹配。

```java
 public Customer findByCustomerId2(int custId){

	String sql = "SELECT * FROM CUSTOMER WHERE CUST_ID = ?";

	Customer customer = (Customer)getJdbcTemplate().queryForObject(
			sql, new Object[] { custId }, 
			new BeanPropertyRowMapper(Customer.class));

	return customer;
} 
```

## 2.查询多行

现在，从数据库中查询或提取多行，并将其转换为一个列表。

## 2.1 手动映射它

在多返回行中，`queryForList()`方法不支持 RowMapper，需要手动映射。

```java
 public List<Customer> findAll(){

	String sql = "SELECT * FROM CUSTOMER";

	List<Customer> customers = new ArrayList<Customer>();

	List<Map> rows = getJdbcTemplate().queryForList(sql);
	for (Map row : rows) {
		Customer customer = new Customer();
		customer.setCustId((Long)(row.get("CUST_ID")));
		customer.setName((String)row.get("NAME"));
		customer.setAge((Integer)row.get("AGE"));
		customers.add(customer);
	}

	return customers;
} 
```

## 2.2 BeanPropertyRowMapper

最简单的解决方案是使用 BeanPropertyRowMapper 类。

```java
 public List<Customer> findAll(){

	String sql = "SELECT * FROM CUSTOMER";

	List<Customer> customers  = getJdbcTemplate().query(sql,
			new BeanPropertyRowMapper(Customer.class));

	return customers;
} 
```

## 3.查询单个值

在此示例中，它显示了如何从数据库中查询或提取单个列值。

## 3.1 单列名称

它展示了如何以字符串形式查询单个列名。

```java
 public String findCustomerNameById(int custId){

	String sql = "SELECT NAME FROM CUSTOMER WHERE CUST_ID = ?";

	String name = (String)getJdbcTemplate().queryForObject(
			sql, new Object[] { custId }, String.class);

	return name;

} 
```

## 3.2 总行数

它展示了如何从数据库中查询总行数。

```java
 public int findTotalCustomer(){

	String sql = "SELECT COUNT(*) FROM CUSTOMER";

	int total = getJdbcTemplate().queryForInt(sql);

	return total;
} 
```

运行它

```java
 package com.mkyong.common;

import java.util.ArrayList;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.mkyong.customer.dao.CustomerDAO;
import com.mkyong.customer.model.Customer;

public class JdbcTemplateApp 
{
    public static void main( String[] args )
    {
    	 ApplicationContext context = 
    		new ClassPathXmlApplicationContext("Spring-Customer.xml");

         CustomerDAO customerDAO = (CustomerDAO) context.getBean("customerDAO");

         Customer customerA = customerDAO.findByCustomerId(1);
         System.out.println("Customer A : " + customerA);

         Customer customerB = customerDAO.findByCustomerId2(1);
         System.out.println("Customer B : " + customerB);

         List<Customer> customerAs = customerDAO.findAll();
         for(Customer cust: customerAs){
         	 System.out.println("Customer As : " + customerAs);
         }

         List<Customer> customerBs = customerDAO.findAll2();
         for(Customer cust: customerBs){
         	 System.out.println("Customer Bs : " + customerBs);
         }

         String customerName = customerDAO.findCustomerNameById(1);
         System.out.println("Customer Name : " + customerName);

         int total = customerDAO.findTotalCustomer();
         System.out.println("Total : " + total);

    }
} 
```

## 结论

JdbcTemplate 类附带了许多有用的重载查询方法。建议在创建自己定制的查询方法之前参考现有的查询方法，因为 Spring 可能已经为您完成了。

## 下载源代码

Download it – [Spring-JdbcTemplate-Querying-Example.zip](http://web.archive.org/web/20190303053044/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-JDBC-Example.zip) (15 KB)[jdbc](http://web.archive.org/web/20190303053044/http://www.mkyong.com/tag/jdbc/) [spring](http://web.archive.org/web/20190303053044/http://www.mkyong.com/tag/spring/)







