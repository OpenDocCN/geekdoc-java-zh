# Spring SimpleJdbcTemplate 查询示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-simplejdbctemplate-querying-examples/>

这里有几个例子来展示如何使用 SimpleJdbcTemplate `query()`方法从数据库中查询或提取数据。在 JdbcTemplate `query()`中，您需要手动将返回的结果转换为所需的对象类型，并传递一个对象数组作为参数。在 SimpleJdbcTemplate 中，它更加用户友好和简单。

**jdbctemplate vesus simplejdbctemplate**
Please compare this [SimpleJdbcTemplate example](http://web.archive.org/web/20190304001019/http://www.mkyong.com/spring/spring-simplejdbctemplate-querying-examples/) with this [JdbcTemplate example](http://web.archive.org/web/20190304001019/http://www.mkyong.com/spring/spring-jdbctemplate-querying-examples/).

## 1.查询单行

这里有两种方法向您展示如何从数据库中查询或提取单个行，并将其转换为模型类。

 ## 1.1 自定义行映射器

一般来说，总是建议实现 RowMapper 接口来创建自定义的 RowMapper 以满足您的需求。

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

```java
 public Customer findByCustomerId(int custId){

	String sql = "SELECT * FROM CUSTOMER WHERE CUST_ID = ?";

	Customer customer = getSimpleJdbcTemplate().queryForObject(
			sql,  new CustomerParameterizedRowMapper(), custId);

	return customer;
} 
```

 ## 1.2 BeanPropertyRowMapper

在 SimpleJdbcTemplate 中，需要使用“ParameterizedBeanPropertyRowMapper”而不是“BeanPropertyRowMapper”。

```java
 public Customer findByCustomerId2(int custId){

	String sql = "SELECT * FROM CUSTOMER WHERE CUST_ID = ?";

	Customer customer = getSimpleJdbcTemplate().queryForObject(sql,
          ParameterizedBeanPropertyRowMapper.newInstance(Customer.class), custId);

	return customer;
} 
```

## 2.查询多行

从数据库中查询或提取多行，并将其转换为列表。

## 2.1 ParameterizedBeanPropertyRowMapper

```java
 public List<Customer> findAll(){

	String sql = "SELECT * FROM CUSTOMER";

	List<Customer> customers = 
		getSimpleJdbcTemplate().query(sql, 
		   ParameterizedBeanPropertyRowMapper.newInstance(Customer.class));

	return customers;
} 
```

## 3.查询单个值

从数据库中查询或提取单个列值。

## 3.1 单列名称

它展示了如何以字符串形式查询单个列名。

```java
 public String findCustomerNameById(int custId){

	String sql = "SELECT NAME FROM CUSTOMER WHERE CUST_ID = ?";

	String name = getSimpleJdbcTemplate().queryForObject(
		sql, String.class, custId);

	return name;

} 
```

## 3.2 总行数

它展示了如何从数据库中查询总行数。

```java
 public int findTotalCustomer(){

	String sql = "SELECT COUNT(*) FROM CUSTOMER";

	int total = getSimpleJdbcTemplate().queryForInt(sql);

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

public class SimpleJdbcTemplateApp 
{
    public static void main( String[] args )
    {
    	 ApplicationContext context = 
    		new ClassPathXmlApplicationContext("Spring-Customer.xml");

         CustomerDAO customerSimpleDAO = 
                (CustomerDAO) context.getBean("customerSimpleDAO");

         Customer customerA = customerSimpleDAO.findByCustomerId(1);
         System.out.println("Customer A : " + customerA);

         Customer customerB = customerSimpleDAO.findByCustomerId2(1);
         System.out.println("Customer B : " + customerB);

         List<Customer> customerAs = customerSimpleDAO.findAll();
         for(Customer cust: customerAs){
         	 System.out.println("Customer As : " + customerAs);
         }

         List<Customer> customerBs = customerSimpleDAO.findAll2();
         for(Customer cust: customerBs){
         	 System.out.println("Customer Bs : " + customerBs);
         }

         String customerName = customerSimpleDAO.findCustomerNameById(1);
         System.out.println("Customer Name : " + customerName);

         int total = customerSimpleDAO.findTotalCustomer();
         System.out.println("Total : " + total);

    }
} 
```

## 结论

SimpleJdbcTemplate 不是 JdbcTemplate 的替代品，它只是 java5 友好的补充。

## 下载源代码

Download it – [Spring-SimpleJdbcTemplate-Querying-Example.zip](http://web.archive.org/web/20190304001019/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-JDBC-Example.zip) (15 KB)[jdbc](http://web.archive.org/web/20190304001019/http://www.mkyong.com/tag/jdbc/) [spring](http://web.archive.org/web/20190304001019/http://www.mkyong.com/tag/spring/)







