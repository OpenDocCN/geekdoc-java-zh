# TestNG–依赖性测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-tutorial-7-dependency-test/>

在 TestNG 中，我们使用`dependOnMethods`和`dependsOnGroups`来实现依赖测试。如果一个从属方法失败，所有后续的测试方法将被跳过，而不是失败。

## 1.dependOnMethods 示例

一个简单的例子，“方法 2()”依赖于“方法 1()”。

**1.1** 如果`method1()`通过，将执行`method2()`。

App.java

```java
 package com.mkyong.testng.examples.dependency;

import org.testng.annotations.Test;

public class App {

	@Test
	public void method1() {
		System.out.println("This is method 1");
	}

	@Test(dependsOnMethods = { "method1" })
	public void method2() {
		System.out.println("This is method 2");
	}

} 
```

输出

```java
 This is method 1
This is method 2
PASSED: method1
PASSED: method2

===============================================
    Default test
    Tests run: 2, Failures: 0, Skips: 0
=============================================== 
```

**1.2** 如果`method1()`失败，`method2()`将被跳过。

App.java

```java
 package com.mkyong.testng.examples.dependency;

import org.testng.annotations.Test;

public class App {

	//This test will be failed.
	@Test
	public void method1() {
		System.out.println("This is method 1");
		throw new RuntimeException();
	}

	@Test(dependsOnMethods = { "method1" })
	public void method2() {
		System.out.println("This is method 2");
	}

} 
```

输出

```java
 This is method 1
FAILED: method1
java.lang.RuntimeException
	at com.mkyong.testng.examples.dependency.App.method1(App.java:10)
	//...

SKIPPED: method2

===============================================
    Default test
    Tests run: 2, Failures: 1, Skips: 1
=============================================== 
```

 ## 2.dependsOnGroups 示例

让我们创建几个测试用例来演示`dependsOnMethods`和`dependsOnGroups`的混合使用。不言自明的见注释。

TestServer.java

```java
 package com.mkyong.testng.examples.dependency;

import org.testng.annotations.Test;

//all methods of this class are belong to "deploy" group.
@Test(groups="deploy")
public class TestServer {

	@Test
	public void deployServer() {
		System.out.println("Deploying Server...");
	}

	//Run this if deployServer() is passed.
	@Test(dependsOnMethods="deployServer")
	public void deployBackUpServer() {
		System.out.println("Deploying Backup Server...");
	}

} 
```

TestDatabase.java

```java
 package com.mkyong.testng.examples.dependency;

import org.testng.annotations.Test;

public class TestDatabase {

	//belong to "db" group, 
	//Run if all methods from "deploy" group are passed.
	@Test(groups="db", dependsOnGroups="deploy")
	public void initDB() {
		System.out.println("This is initDB()");
	}

	//belong to "db" group,
	//Run if "initDB" method is passed.
	@Test(dependsOnMethods = { "initDB" }, groups="db")
	public void testConnection() {
		System.out.println("This is testConnection()");
	}

} 
```

TestApp.java

```java
 package com.mkyong.testng.examples.dependency;

import org.testng.annotations.Test;

public class TestApp {

	//Run if all methods from "deploy" and "db" groups are passed.
	@Test(dependsOnGroups={"deploy","db"})
	public void method1() {
		System.out.println("This is method 1");
		//throw new RuntimeException();
	}

	//Run if method1() is passed.
	@Test(dependsOnMethods = { "method1" })
	public void method2() {
		System.out.println("This is method 2");
	}

} 
```

创建一个 XML 文件并一起测试它们。

testng.xml

```java
 <!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >

<suite name="TestDependency">

  <test name="TestCase1">

	<classes>
	  <class 
		name="com.mkyong.testng.examples.dependency.TestApp">
	  </class>
	  <class 
		name="com.mkyong.testng.examples.dependency.TestDatabase">
	  </class>
	  <class 
		name="com.mkyong.testng.examples.dependency.TestServer">
	  </class>
	</classes>

  </test>

</suite> 
```

输出

```java
 Deploying Server...
Deploying Backup Server...
This is initDB()
This is testConnection()
This is method 1
This is method 2

===============================================
TestDependency
Total tests run: 6, Failures: 0, Skips: 0
=============================================== 
```

![testng-dependency-test](img/474a7e43e0960f11896001aeddf30f7b.png) ## 参考

1.  [测试依赖方法](http://web.archive.org/web/20190227120346/http://testng.org/doc/documentation-main.html#dependent-methods)

[dependency test](http://web.archive.org/web/20190227120346/http://www.mkyong.com/tag/dependency-test/) [testng](http://web.archive.org/web/20190227120346/http://www.mkyong.com/tag/testng/)







