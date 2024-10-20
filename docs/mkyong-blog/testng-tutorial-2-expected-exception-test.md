# TestNG–预期异常测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-tutorial-2-expected-exception-test/>

在本教程中，我们将向您展示如何使用 TestNG `expectedExceptions`来测试代码中预期的异常抛出。

## 1.运行时异常

这个例子向你展示了如何测试一个运行时异常。如果方法`divisionWithException ()`抛出一个运行时异常——`ArithmeticException`，它将被传递。

TestRuntime.java

```java
 package com.mkyong.testng.examples.exception;

import org.testng.annotations.Test;

public class TestRuntime {

	@Test(expectedExceptions = ArithmeticException.class)
	public void divisionWithException() {
		int i = 1 / 0;
	}

} 
```

上述单元测试将通过。

 ## 2.检查异常

检查一个简单的业务对象，保存和更新方法，如果出错，抛出自定义的检查异常。

OrderBo.java

```java
 package com.mkyong.testng.project.order;

public class OrderBo {

  public void save(Order order) throws OrderSaveException {

	if (order == null) {
	  throw new OrderSaveException("Order is empty!");
	}
	// persist it
  }

  public void update(Order order) throws OrderUpdateException, OrderNotFoundException {

	if (order == null) {
	  throw new OrderUpdateException("Order is empty!");
	}

	// If order is not available in the database
	throw new OrderNotFoundException("Order is not exists");

  }
} 
```

测试预期异常的示例。

TestCheckedException.java

```java
 package com.mkyong.testng.examples.exception;

import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;

import com.mkyong.testng.project.order.Order;
import com.mkyong.testng.project.order.OrderBo;
import com.mkyong.testng.project.order.OrderNotFoundException;
import com.mkyong.testng.project.order.OrderSaveException;
import com.mkyong.testng.project.order.OrderUpdateException;

public class TestCheckedException {

  OrderBo orderBo;
  Order data;

  @BeforeTest
  void setup() {
	orderBo = new OrderBo();

	data = new Order();
	data.setId(1);
	data.setCreatedBy("mkyong");
  }

  @Test(expectedExceptions = OrderSaveException.class)
  public void throwIfOrderIsNull() throws OrderSaveException {
	orderBo.save(null);
  }

  /*
   * Example : Multiple expected exceptions
   * Test is success if either of the exception is thrown
   */
  @Test(expectedExceptions = { OrderUpdateException.class, OrderNotFoundException.class })
  public void throwIfOrderIsNotExists() throws OrderUpdateException, OrderNotFoundException {
	orderBo.update(data);
  }

} 
```

上述单元测试将通过。

 ## 下载源代码

Download it – [TestNG-Example-Excepted-Exception.zip](http://web.archive.org/web/20190227120235/http://www.mkyong.com/wp-content/uploads/2009/05/TestNG-Example-Excepted-Exception.zip) (11 kb)

## 参考

1.  [TestNG 预期异常 JavaDoc](http://web.archive.org/web/20190227120235/http://testng.org/javadoc/org/testng/annotations/ExpectedExceptions.html)

[expected exception](http://web.archive.org/web/20190227120235/http://www.mkyong.com/tag/expected-exception/) [testng](http://web.archive.org/web/20190227120235/http://www.mkyong.com/tag/testng/)







