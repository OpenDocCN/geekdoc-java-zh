# TestNG+Selenium–负载测试示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-selenium-load-testing-example/>

![testng-selenium-load-test](img/9c994d7cc9ebc98204428242eebde94d.png)

在本教程中，我们将向您展示如何使用@Test 属性`invocationCount`和`threadPoolSize`在网站上执行负载测试或压力测试。

使用的工具:

1.  测试 6.8.7
2.  硒
3.  maven3

我们正在使用 Selenium 库来自动化浏览器访问网站。

## 1.项目依赖性

获取 TestNG 和 Selenium 库。

pom.xml

```java
 <properties>
	<testng.version>6.8.7</testng.version>
	<selenium.version>2.39.0</selenium.version>
  </properties>

  <dependencies>
	<dependency>
		<groupId>org.testng</groupId>
		<artifactId>testng</artifactId>
		<version>${testng.version}</version>
		<scope>test</scope>
	</dependency>
	<dependency>
		<groupId>org.seleniumhq.selenium</groupId>
		<artifactId>selenium-java</artifactId>
		<version>${selenium.version}</version>
	</dependency>
   <dependencies> 
```

 ## 2.@Test(invocationCount=？)

这个`invocationCount`决定了 TestNG 应该运行这个测试方法多少次。

*例 2.1*

```java
 @Test(invocationCount = 10)
  public void repeatThis() {
    //...
  } 
```

输出—`repeatThis()`方法将运行 10 次。

```java
 PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis 
```

*示例 2.2*–使用 Selenium 打开 Firefox 浏览器并加载“Google.com”。这个测试是为了确保页面标题始终是“Google”。

```java
 package com.mkyong.testng.examples.loadtest;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.Assert;
import org.testng.annotations.Test;

public class TestMultipleThreads {

	@Test(invocationCount = 5)
	public void loadTestThisWebsite() {

		WebDriver driver = new FirefoxDriver();		
		driver.get("http://www.google.com");
		System.out.println("Page Title is " + driver.getTitle());
		Assert.assertEquals("Google", driver.getTitle());
		driver.quit();

	}
} 
```

输出–你会注意到 Firefox 浏览器会提示退出并关闭 5 次。

```java
 Page Title is Google
Page Title is Google
Page Title is Google
Page Title is Google
Page Title is Google
PASSED: loadTestThisWebsite
PASSED: loadTestThisWebsite
PASSED: loadTestThisWebsite
PASSED: loadTestThisWebsite
PASSED: loadTestThisWebsite 
```

 ## 3.@Test(invocationCount =？，threadPoolSize =？)

`threadPoolSize`属性告诉 TestNG 创建一个线程池，通过多线程运行测试方法。有了线程池，将大大减少测试方法的运行时间。

*例 3.1*–启动一个线程池，包含 3 个线程，运行测试方法 3 次。

```java
 @Test(invocationCount = 3, threadPoolSize = 3)
  public void testThreadPools() {

	System.out.printf("Thread Id : %s%n", Thread.currentThread().getId());

  } 
```

输出——测试方法运行 3 次，每次都接收到自己的线程。

```java
 [ThreadUtil] Starting executor timeOut:0ms workers:3 threadPoolSize:3
Thread Id : 10
Thread Id : 12
Thread Id : 11
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools 
```

*例 3.2*–启动一个线程池，包含 3 个线程，运行测试方法 10 次。

```java
 @Test(invocationCount = 10, threadPoolSize = 3)
  public void testThreadPools() {

	System.out.printf("Thread Id : %s%n", Thread.currentThread().getId());

  } 
```

输出–测试方法运行 10 次，线程被重用。

```java
 [ThreadUtil] Starting executor timeOut:0ms workers:10 threadPoolSize:3
Thread Id : 10
Thread Id : 11
Thread Id : 12
Thread Id : 10
Thread Id : 11
Thread Id : 12
Thread Id : 10
Thread Id : 11
Thread Id : 12
Thread Id : 10
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools 
```

## 4.负载测试示例

通过结合 TestNG 多线程和 Selenium 强大的浏览器自动化。您可以创建一个简单而强大的负载测试，如下所示:

TestMultipleThreads.java

```java
 package com.mkyong.testng.examples.loadtest;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.Assert;
import org.testng.annotations.Test;

public class TestMultipleThreads {

  @Test(invocationCount = 100, threadPoolSize = 5)
  public void loadTest() {

	System.out.printf("%n[START] Thread Id : %s is started!", 
                                  Thread.currentThread().getId());

	WebDriver driver = new FirefoxDriver();
	driver.get("http://yourwebsite.com");

	//perform whatever actions, like login, submit form or navigation

	System.out.printf("%n[END] Thread Id : %s", 
                                  Thread.currentThread().getId());

	driver.quit();

  }
} 
```

输出–上面的测试将启动一个 5 线程池，并向一个指定的网站发送 100 个 URL 请求。

```java
 [ThreadUtil] Starting executor timeOut:0ms workers:100 threadPoolSize:5

[START] Thread Id : 11 is started!
[START] Thread Id : 14 is started!
[START] Thread Id : 10 is started!
[START] Thread Id : 12 is started!
[START] Thread Id : 13 is started!
[END] Thread Id : 11
[START] Thread Id : 11 is started!
[END] Thread Id : 10
[START] Thread Id : 10 is started!
[END] Thread Id : 13
[START] Thread Id : 13 is started!
[END] Thread Id : 14
[START] Thread Id : 14 is started!
[END] Thread Id : 12
[START] Thread Id : 12 is started!
[END] Thread Id : 13
...... 
```

**FAQs**
Q : For load testing with Selenium and TestNG, why only one browser is prompts out at a time?
A : Your test method is completed too fast, try putting a `Thread.sleep(5000)` to delay the execution, now, you should notice multiple browsers prompt out simultaneously. (For demonstration purpose only).

## 下载源代码

Download – [TestNG-LoadTest-Example.zip](http://web.archive.org/web/20190227120419/http://www.mkyong.com/wp-content/uploads/2014/01/TestNG-LoadTest-Example.zip) (15 kb)

## 参考

1.  [硒网站](http://web.archive.org/web/20190227120419/http://docs.seleniumhq.org/)
2.  [维基百科:负载测试](http://web.archive.org/web/20190227120419/http://en.wikipedia.org/wiki/Load_testing)

[load test](http://web.archive.org/web/20190227120419/http://www.mkyong.com/tag/load-test/) [selenium](http://web.archive.org/web/20190227120419/http://www.mkyong.com/tag/selenium/) [testng](http://web.archive.org/web/20190227120419/http://www.mkyong.com/tag/testng/)







