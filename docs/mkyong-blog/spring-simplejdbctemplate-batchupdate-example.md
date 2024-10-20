# spring simple JDBC template batch update()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-simplejdbctemplate-batchupdate-example/>

在本教程中，我们将向您展示如何在 SimpleJdbcTemplate 类中使用`batchUpdate()`。

参见 SimpleJdbcTemplate 类中的`batchUpdate()`示例。

```java
 //insert batch example
public void insertBatch(final List<Customer> customers){
	String sql = "INSERT INTO CUSTOMER " +
		"(CUST_ID, NAME, AGE) VALUES (?, ?, ?)";

	List<Object[]> parameters = new ArrayList<Object[]>();

	for (Customer cust : customers) {
        parameters.add(new Object[] {cust.getCustId(), 
            cust.getName(), cust.getAge()}
        );
    }
    getSimpleJdbcTemplate().batchUpdate(sql, parameters);        
} 
```

或者，您可以直接执行 SQL。

```java
 //insert batch example with SQL
public void insertBatchSQL(final String sql){

	getJdbcTemplate().batchUpdate(new String[]{sql});

} 
```

Spring 的 bean 配置文件

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerSimpleDAO" 
        class="com.mkyong.customer.dao.impl.SimpleJdbcCustomerDAO">

		<property name="dataSource" ref="dataSource" />
	</bean>

	<bean id="dataSource" 
        class="org.springframework.jdbc.datasource.DriverManagerDataSource">

		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/mkyongjava" />
		<property name="username" value="root" />
		<property name="password" value="password" />
	</bean>

</beans> 
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

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
    		new ClassPathXmlApplicationContext("Spring-Customer.xml");

        CustomerDAO customerSimpleDAO = 
                      (CustomerDAO) context.getBean("customerSimpleDAO");

        Customer customer1 = new Customer(1, "mkyong1",21);
        Customer customer3 = new Customer(2, "mkyong2",22);
        Customer customer2 = new Customer(3, "mkyong3",23);

        List<Customer>customers = new ArrayList<Customer>();
        customers.add(customer1);
        customers.add(customer2);
        customers.add(customer3);

        customerSimpleDAO.insertBatch(customers);

        String sql = "UPDATE CUSTOMER SET NAME ='BATCHUPDATE'";
        customerSimpleDAO.insertBatchSQL(sql);

    }
} 
```

在本例中，您将插入三个客户的记录，并批量更新所有客户的姓名。

## 下载源代码

Download it – [Spring-SimpleJdbcTemplate-batchUpdate-Example.zip](http://web.archive.org/web/20190310101928/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-JDBC-Example.zip) (15 KB)[jdbc](http://web.archive.org/web/20190310101928/http://www.mkyong.com/tag/jdbc/) [spring](http://web.archive.org/web/20190310101928/http://www.mkyong.com/tag/spring/)![](img/e2d83b8cab0c8439728b378ab3126b0c.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190310101928/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3816">







