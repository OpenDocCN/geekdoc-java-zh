# Spring EL 运算符示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-el-operators-example/>

Spring EL 支持大多数标准的数学、逻辑或关系运算符。举个例子，

1.  **关系运算符**–等于(==，eq)，不等于(！=，ne)，小于(【T2，lt)，小于或等于(< =，le)，大于(【T4，gt)，大于或等于(> =，ge)。
2.  **逻辑运算符**–and、or、and not(！).
3.  **数学运算符**–加法(+)、减法(-)、乘法(*)、除法(/)、模数(%)和指数幂(^).

## 注释中的弹簧 EL

这个例子演示了 SpEL 中操作符的使用。

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("customerBean")
public class Customer {

	//Relational operators

	@Value("#{1 == 1}") //true
	private boolean testEqual;

	@Value("#{1 != 1}") //false
	private boolean testNotEqual;

	@Value("#{1 < 1}") //false
	private boolean testLessThan;

	@Value("#{1 <= 1}") //true
	private boolean testLessThanOrEqual;

	@Value("#{1 > 1}") //false
	private boolean testGreaterThan;

	@Value("#{1 >= 1}") //true
	private boolean testGreaterThanOrEqual;

	//Logical operators , numberBean.no == 999

	@Value("#{numberBean.no == 999 and numberBean.no < 900}") //false
	private boolean testAnd;

	@Value("#{numberBean.no == 999 or numberBean.no < 900}") //true
	private boolean testOr;

	@Value("#{!(numberBean.no == 999)}") //false
	private boolean testNot;

	//Mathematical operators

	@Value("#{1 + 1}") //2.0
	private double testAdd;

	@Value("#{'1' + '@' + '1'}") //1@1
	private String testAddString;

	@Value("#{1 - 1}") //0.0
	private double testSubtraction;

	@Value("#{1 * 1}") //1.0
	private double testMultiplication;

	@Value("#{10 / 2}") //5.0
	private double testDivision;

	@Value("#{10 % 10}") //0.0
	private double testModulus ;

	@Value("#{2 ^ 2}") //4.0
	private double testExponentialPower;

	@Override
	public String toString() {
		return "Customer [testEqual=" + testEqual + ", testNotEqual="
				+ testNotEqual + ", testLessThan=" + testLessThan
				+ ", testLessThanOrEqual=" + testLessThanOrEqual
				+ ", testGreaterThan=" + testGreaterThan
				+ ", testGreaterThanOrEqual=" + testGreaterThanOrEqual
				+ ", testAnd=" + testAnd + ", testOr=" + testOr + ", testNot="
				+ testNot + ", testAdd=" + testAdd + ", testAddString="
				+ testAddString + ", testSubtraction=" + testSubtraction
				+ ", testMultiplication=" + testMultiplication
				+ ", testDivision=" + testDivision + ", testModulus="
				+ testModulus + ", testExponentialPower="
				+ testExponentialPower + "]";
	}

} 
```

```java
 package com.mkyong.core;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("numberBean")
public class Number {

	@Value("999")
	private int no;

	public int getNo() {
		return no;
	}

	public void setNo(int no) {
		this.no = no;
	}

} 
```

*运行它*

```java
 Customer obj = (Customer) context.getBean("customerBean");
       System.out.println(obj); 
```

*输出*

```java
 Customer [
	testEqual=true, 
	testNotEqual=false, 
	testLessThan=false, 
	testLessThanOrEqual=true, 
	testGreaterThan=false, 
	testGreaterThanOrEqual=true, 
	testAnd=false, 
	testOr=true, 
	testNot=false, 
	testAdd=2.0, 
	testAddString=1@1, 
	testSubtraction=0.0, 
	testMultiplication=1.0, 
	testDivision=5.0, 
	testModulus=0.0, 
	testExponentialPower=4.0
] 
```

 ## XML 中的 Spring EL

请参见 bean 定义 XML 文件中的等效版本。在 XML 中，像“**小于**这样的符号总是不被支持的，相反，你应该使用上面所示的文本等价符号，例如(' **<** ' = ' **lt** ')和(' **< =** ' = ' **le** ')。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="customerBean" class="com.mkyong.core.Customer">

	  <property name="testEqual" value="#{1 == 1}" />
	  <property name="testNotEqual" value="#{1 != 1}" />
	  <property name="testLessThan" value="#{1 lt 1}" />
	  <property name="testLessThanOrEqual" value="#{1 le 1}" />
	  <property name="testGreaterThan" value="#{1 > 1}" />
	  <property name="testGreaterThanOrEqual" value="#{1 >= 1}" />

	  <property name="testAnd" value="#{numberBean.no == 999 and numberBean.no lt 900}" />
	  <property name="testOr" value="#{numberBean.no == 999 or numberBean.no lt 900}" />
	  <property name="testNot" value="#{!(numberBean.no == 999)}" />

	  <property name="testAdd" value="#{1 + 1}" />
	  <property name="testAddString" value="#{'1' + '@' + '1'}" />
	  <property name="testSubtraction" value="#{1 - 1}" />
	  <property name="testMultiplication" value="#{1 * 1}" />
	  <property name="testDivision" value="#{10 / 2}" />
	  <property name="testModulus" value="#{10 % 10}" />
	  <property name="testExponentialPower" value="#{2 ^ 2}" />

	</bean>

	<bean id="numberBean" class="com.mkyong.core.Number">
		<property name="no" value="999" />
	</bean>

</beans> 
```

 ## 下载源代码

Download It - [Spring3-EL-Operator-Example.zip](http://web.archive.org/web/20190225100531/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Operator-Example.zip) (7 KB)

## 参考

1.  [官方春联运营商参考文献](http://web.archive.org/web/20190225100531/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/expressions.html#d0e11931)

[spring el](http://web.archive.org/web/20190225100531/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190225100531/http://www.mkyong.com/tag/spring3/)







