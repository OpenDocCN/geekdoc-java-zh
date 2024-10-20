# Hibernate 中的 Spring AOP 事务管理

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-aop-transaction-management-in-hibernate/>

需要事务管理来保证数据库中数据的完整性和一致性。Spring 的 AOP 技术允许开发者声明性地管理事务。

下面的例子展示了如何用 Spring AOP 管理 Hibernate 事务。

*P.S 很多 Hibernate 和 Spring 的配置文件都是隐藏的，只显示了一些重要的文件，如果你想亲自动手，可以在文末下载完整的项目。*

## 1.表格创建

MySQL 表脚本，一个**产品**表和一个**产品现存量**表。

```java
 CREATE TABLE  `mkyong`.`product` (
  `PRODUCT_ID` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `PRODUCT_CODE` varchar(20) NOT NULL,
  `PRODUCT_DESC` varchar(255) NOT NULL,
  PRIMARY KEY (`PRODUCT_ID`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE TABLE  `mkyong`.`product_qoh` (
  `QOH_ID` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `PRODUCT_ID` bigint(20) unsigned NOT NULL,
  `QTY` int(10) unsigned NOT NULL,
  PRIMARY KEY (`QOH_ID`),
  KEY `FK_product_qoh_product_id` (`PRODUCT_ID`),
  CONSTRAINT `FK_product_qoh_product_id` FOREIGN KEY (`PRODUCT_ID`) 
  REFERENCES `product` (`PRODUCT_ID`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8; 
```

## 2.产品业务对象

在这个' **productBo** '实现中， **save()** 方法将通过 **'productDao** '类向' **product** 表中插入一条记录，并通过' **productQohBo** '类向' **productQoh** 表中插入一条现有量记录。

```java
 package com.mkyong.product.bo.impl;

import com.mkyong.product.bo.ProductBo;
import com.mkyong.product.bo.ProductQohBo;
import com.mkyong.product.dao.ProductDao;
import com.mkyong.product.model.Product;
import com.mkyong.product.model.ProductQoh;

public class ProductBoImpl implements ProductBo{

	ProductDao productDao;
	ProductQohBo productQohBo;

	public void setProductDao(ProductDao productDao) {
		this.productDao = productDao;
	}

	public void setProductQohBo(ProductQohBo productQohBo) {
		this.productQohBo = productQohBo;
	}

	//this method need to be transactional
	public void save(Product product, int qoh){

		productDao.save(product);
		System.out.println("Product Inserted");

		ProductQoh productQoh = new ProductQoh();
		productQoh.setProductId(product.getProductId());
		productQoh.setQty(qoh);

		productQohBo.save(productQoh);
		System.out.println("ProductQoh Inserted");
	}
} 
```

Spring 的 bean 配置文件。

```java
 <!-- Product business object -->
   <bean id="productBo" class="com.mkyong.product.bo.impl.ProductBoImpl" >
   	<property name="productDao" ref="productDao" />
   	<property name="productQohBo" ref="productQohBo" />
   </bean>

   <!-- Product Data Access Object -->
   <bean id="productDao" class="com.mkyong.product.dao.impl.ProductDaoImpl" >
   	<property name="sessionFactory" ref="sessionFactory"></property>
   </bean> 
```

运行它

```java
 Product product = new Product();
    product.setProductCode("ABC");
    product.setProductDesc("This is product ABC");

    ProductBo productBo = (ProductBo)appContext.getBean("productBo");
    productBo.save(product, 100); 
```

假设 **save()** 不具有事务特性，如果 **productQohBo.save()** 抛出异常，您将只在' **product** 表中插入一条记录，而不会在' **productQoh** 表中插入任何记录。这是一个严重的问题，会破坏数据库中的数据一致性。

## 3.事务管理

为 Hibernate 事务声明了一个'**transaction interceptor**bean 和一个'**Hibernate transaction manager**，并传递了必要的属性。

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

    <bean id="transactionInterceptor" 
       class="org.springframework.transaction.interceptor.TransactionInterceptor">
	<property name="transactionManager" ref="transactionManager" />
	<property name="transactionAttributes">
	   <props>
		<prop key="save">PROPAGATION_REQUIRED</prop>
	   </props>
	</property>
    </bean>

    <bean id="transactionManager" 
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
	  <property name="dataSource" ref="dataSource" />
	  <property name="sessionFactory" ref="sessionFactory" />
    </bean>

</beans> 
```

##### 交易属性

在事务拦截器中，您必须定义应该使用哪个事务属性'**传播行为**。意思是如果一个事务性的 **'ProductBoImpl.save()** '方法被另一个方法' **productQohBo.save()** '调用，那么事务应该如何传播？它应该继续在现有事务中运行吗？或者为它自己开始新的事务。

Spring 支持 7 种类型的传播:

*   **PROPAGATION _ REQUIRED**–支持当前交易；如果不存在，请创建一个新的。
*   **PROPAGATION _ SUPPORTS**–支持当前交易；如果不存在，则以非事务方式执行。
*   **PROPAGATION _ MANDATORY**–支持当前交易；如果不存在当前事务，则引发异常。
*   **PROPAGATION _ REQUIRES _ NEW**–创建新事务，如果当前事务存在，则暂停当前事务。
*   **PROPAGATION _ NOT _ SUPPORTED**–不支持当前事务；而是总是以非事务方式执行。
*   **PROPAGATION _ NEVER**–不支持当前交易；如果当前事务存在，则引发异常。
*   **PROPAGATION _ NESTED**–如果当前事务存在，则在嵌套事务中执行，类似于 PROPAGATION_REQUIRED else。

在大多数情况下，您可能只需要使用 PROPAGATION_REQUIRED。

此外，您还必须定义方法来支持这个事务属性。方法名支持通配符格式，一个 **save*** 将匹配所有以 save(…)开头的方法名。

##### 事务管理程序

在 Hibernate 事务中，需要使用**HibernateTransactionManager**。如果只处理纯 JDBC，使用**DataSourceTransactionManager**；而 JTA 则使用 **JtaTransactionManager** 。

## 4.代理工厂 Bean

为 **ProductBo** 创建一个新的代理工厂 bean，并设置“**拦截器名称**属性。

```java
 <!-- Product business object -->
   <bean id="productBo" class="com.mkyong.product.bo.impl.ProductBoImpl" >
   	<property name="productDao" ref="productDao" />
   	<property name="productQohBo" ref="productQohBo" />
   </bean>

   <!-- Product Data Access Object -->
   <bean id="productDao" class="com.mkyong.product.dao.impl.ProductDaoImpl" >
   	<property name="sessionFactory" ref="sessionFactory"></property>
   </bean>

   <bean id="productBoProxy"
	class="org.springframework.aop.framework.ProxyFactoryBean">
	<property name="target" ref="productBo" />
	<property name="interceptorNames">
		<list>
			<value>transactionInterceptor</value>
		</list>
	</property>
  </bean> 
```

运行它

```java
 Product product = new Product();
    product.setProductCode("ABC");
    product.setProductDesc("This is product ABC");

    ProductBo productBo = (ProductBo)appContext.getBean("productBoProxy");
    productBo.save(product, 100); 
```

获取您的代理 bean ' **productBoProxy** '，并且您的 **save()** 方法现在支持事务性，在 **productBo.save()** 方法中的任何异常都将导致整个事务回滚，不会有数据插入数据库。

## 下载源代码

Download it – [Spring-Hibernate-Transaction-Example.zip](http://web.archive.org/web/20220508112627/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Hibernate-Transaction-Example.zip)

## 参考

1.  [http://static . springsource . org/spring/docs/2.5 . x/API/org/spring framework/transaction/transaction definition . html](http://web.archive.org/web/20220508112627/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/transaction/TransactionDefinition.html)
2.  [http://static . springsource . org/spring/docs/2.5 . x/reference/transaction . html](http://web.archive.org/web/20220508112627/http://static.springsource.org/spring/docs/2.5.x/reference/transaction.html)

<input type="hidden" id="mkyong-current-postId" value="4135">