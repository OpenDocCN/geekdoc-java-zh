# Spring EL 三元运算符(if-then-else)示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-el-ternary-operator-if-then-else-example/>

Spring EL 支持**三元运算符**，执行 **if then else** 条件检查。举个例子，

```java
 condition ? true : false 
```

## 注释中的弹簧 EL

带**@值**标注的 Spring EL 三元运算符。在本例中，如果“ **itemBean.qtyOnHand** ”小于 100，则将“ **customerBean.warning** ”设置为 true，否则设置为 false。

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("customerBean")
public class Customer {

	@Value("#{itemBean.qtyOnHand < 100 ? true : false}")
	private boolean warning;

	public boolean isWarning() {
		return warning;
	}

	public void setWarning(boolean warning) {
		this.warning = warning;
	}

	@Override
	public String toString() {
		return "Customer [warning=" + warning + "]";
	}

} 
```

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("itemBean")
public class Item {

	@Value("99")
	private int qtyOnHand;

	public int getQtyOnHand() {
		return qtyOnHand;
	}

	public void setQtyOnHand(int qtyOnHand) {
		this.qtyOnHand = qtyOnHand;
	}

} 
```

*输出*

```java
 Customer [warning=true] 
```

 ## XML 中的 Spring EL

请参见 bean 定义 XML 文件中的等效版本。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="customerBean" class="com.mkyong.core.Customer">
		<property name="warning" 
                          value="#{itemBean.qtyOnHand < 100 ? true : false}" />
	</bean>

	<bean id="itemBean" class="com.mkyong.core.Item">
		<property name="qtyOnHand" value="99" />
	</bean>

</beans> 
```

*输出*

```java
 Customer [warning=true] 
```

在 XML 中，需要将小于运算符“ **<** ”替换为“**&lt；**”。

 ## 下载源代码

Download It - [Spring3-EL-Ternary-Operator-Example.zip](http://web.archive.org/web/20190304000811/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Ternary-Operator-Example.zip) (6 KB)[spring el](http://web.archive.org/web/20190304000811/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190304000811/http://www.mkyong.com/tag/spring3/)







