# TestNG–参数测试(XML 和@DataProvider)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-tutorial-6-parameterized-test/>

在本教程中，我们将向您展示如何通过 XML `@Parameters`或`@DataProvider`将参数传递给`@Test`方法。

## 1.用 XML 传递参数

在这个例子中，属性 filename 从`testng.xml`传递，并通过`@Parameters`注入到方法中。

TestParameterXML.java

```java
 package com.mkyong.testng.examples.parameter;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.util.Properties;

import org.testng.annotations.Parameters;
import org.testng.annotations.Test;

public class TestParameterXML {

	Connection con;

	@Test
	@Parameters({ "dbconfig", "poolsize" })
	public void createConnection(String dbconfig, int poolsize) {

		System.out.println("dbconfig : " + dbconfig);
		System.out.println("poolsize : " + poolsize);

		Properties prop = new Properties();
		InputStream input = null;

		try {
		  //get properties file from project classpath
		  input = getClass().getClassLoader().getResourceAsStream(dbconfig);

		  prop.load(input);

		  String drivers = prop.getProperty("jdbc.driver");
		  String connectionURL = prop.getProperty("jdbc.url");
		  String username = prop.getProperty("jdbc.username");
		  String password = prop.getProperty("jdbc.password");

		  System.out.println("drivers : " + drivers);
		  System.out.println("connectionURL : " + connectionURL);
		  System.out.println("username : " + username);
		  System.out.println("password : " + password);

		  Class.forName(drivers);
		  con = DriverManager.getConnection(connectionURL, username, password);

		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (input != null) {
				try {
					input.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}

	}

} 
```

db.properties

```java
 jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mkyongserver
jdbc.username=mkyong
jdbc.password=password 
```

testng.xml

```java
 <!DOCTYPE suite SYSTEM "http://beust.com/testng/testng-1.0.dtd" >
<suite name="test-parameter">

    <test name="example1">

	<parameter name="dbconfig" value="db.properties" />
	<parameter name="poolsize" value="10" />

	<classes>
	  <class name="com.mkyong.testng.examples.parameter.TestParameterXML" />
	</classes>

    </test>

</suite> 
```

输出

```java
 dbconfig : db.properties
poolsize : 10
drivers : com.mysql.jdbc.Driver
connectionURL : jdbc:mysql://localhost:3306/mkyongserver
username : mkyong
password : password 
```

 ## 2.用@DataProvider 传递参数

**2.1** 回顾一个简单的`@DataProvider`例子，传递一个`int`参数。

TestParameterDataProvider.java

```java
package com.mkyong.testng.examples.parameter;

import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider {

	@Test(dataProvider = "provideNumbers")
	public void test(int number, int expected) {
		Assert.assertEquals(number + 10, expected);
	}

	@DataProvider(name = "provideNumbers")
	public Object[][] provideData() {

		return new Object[][] { 
			{ 10, 20 }, 
			{ 100, 110 }, 
			{ 200, 210 } 
		};
	}

}

```

输出

```java
 PASSED: test(10, 20)
PASSED: test(100, 110)
PASSED: test(200, 210) 
```

**2.2**`@DataProvider`是传递一个`object`参数的支持。下面的例子展示了如何传递一个`Map`对象作为参数。

TestParameterDataProvider.java

```java
 package com.mkyong.testng.examples.parameter;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider {

	@Test(dataProvider = "dbconfig")
	public void testConnection(Map<String, String> map) {

		for (Map.Entry<String, String> entry : map.entrySet()) {
		  System.out.println("[Key] : " + entry.getKey() 
                              + " [Value] : " + entry.getValue());
		}

	}

	@DataProvider(name = "dbconfig")
	public Object[][] provideDbConfig() {
		Map<String, String> map = readDbConfig();
		return new Object[][] { { map } };
	}

	public Map<String, String> readDbConfig() {

		Properties prop = new Properties();
		InputStream input = null;
		Map<String, String> map = new HashMap<String, String>();

		try {
		  input = getClass().getClassLoader().getResourceAsStream("db.properties");

		  prop.load(input);

		  map.put("jdbc.driver", prop.getProperty("jdbc.driver"));
		  map.put("jdbc.url", prop.getProperty("jdbc.url"));
		  map.put("jdbc.username", prop.getProperty("jdbc.username"));
		  map.put("jdbc.password", prop.getProperty("jdbc.password"));

		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (input != null) {
				try {
					input.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}

		return map;

	}

} 
```

输出

```java
 [Key] : jdbc.url [Value] : jdbc:mysql://localhost:3306/mkyongserver
[Key] : jdbc.username [Value] : mkyong
[Key] : jdbc.driver [Value] : com.mysql.jdbc.Driver
[Key] : jdbc.password [Value] : password
PASSED: testConnection({jdbc.url=jdbc:mysql://localhost:3306/mkyongserver, 
jdbc.username=mkyong, jdbc.driver=com.mysql.jdbc.Driver, jdbc.password=password}) 
```

 ## 3.@DataProvider +方法

这个例子向您展示了如何根据测试方法的名称来传递不同的参数。

TestParameterDataProvider.java

```java
 package com.mkyong.testng.examples.parameter;

import java.lang.reflect.Method;
import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider {

	@Test(dataProvider = "dataProvider")
	public void test1(int number, int expected) {
		Assert.assertEquals(number, expected);
	}

	@Test(dataProvider = "dataProvider")
	public void test2(String email, String expected) {
		Assert.assertEquals(email, expected);
	}

	@DataProvider(name = "dataProvider")
	public Object[][] provideData(Method method) {

		Object[][] result = null;

		if (method.getName().equals("test1")) {
			result = new Object[][] {
				{ 1, 1 }, { 200, 200 } 
			};
		} else if (method.getName().equals("test2")) {
			result = new Object[][] { 
				{ "test@gmail.com", "test@gmail.com" }, 
				{ "test@yahoo.com", "test@yahoo.com" } 
			};
		}

		return result;

	}

} 
```

输出

```java
 PASSED: test1(1, 1)
PASSED: test1(200, 200)
PASSED: test2("test@gmail.com", "test@gmail.com")
PASSED: test2("test@yahoo.com", "test@yahoo.com") 
```

## 4\. @DataProvider + ITestContext

在 TestNG 中，我们可以使用`org.testng.ITestContext`来确定调用当前测试方法的运行时参数。在最后一个例子中，我们将向您展示如何根据包含的组名来传递参数。

TestParameterDataProvider.java

```java
 package com.mkyong.testng.examples.parameter;

import org.testng.Assert;
import org.testng.ITestContext;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider {

	@Test(dataProvider = "dataProvider", groups = {"groupA"})
	public void test1(int number) {
		Assert.assertEquals(number, 1);
	}

	@Test(dataProvider = "dataProvider", groups = "groupB")
	public void test2(int number) {
		Assert.assertEquals(number, 2);
	}

	@DataProvider(name = "dataProvider")
	public Object[][] provideData(ITestContext context) {

		Object[][] result = null;

		//get test name
		//System.out.println(context.getName());

		for (String group : context.getIncludedGroups()) {

			System.out.println("group : " + group);

			if ("groupA".equals(group)) {
				result = new Object[][] { { 1 } };
				break;
			}

		}

		if (result == null) {
			result = new Object[][] { { 2 } };
		}
		return result;

	}

} 
```

testng.xml

```java
 <!DOCTYPE suite SYSTEM "http://beust.com/testng/testng-1.0.dtd" >
<suite name="test-parameter">

  <test name="example1">

	<groups>
		<run>
			<include name="groupA" />
		</run>
	</groups>

	<classes>
	   <class
	    name="com.mkyong.testng.examples.parameter.TestParameterDataProvider" />
	</classes>

  </test>

</suite> 
```

输出

```java
 group : groupA 
```

完成了。

## 参考

1.  [TestNG @DataProvider](http://web.archive.org/web/20190228162831/http://testng.org/doc/documentation-main.html#parameters-dataproviders)
2.  [TestNG ITestContext JavaDoc](http://web.archive.org/web/20190228162831/http://testng.org/javadocs/org/testng/ITestContext.html)
3.  [使用 JDBC 驱动程序连接 MySQL】](http://web.archive.org/web/20190228162831/http://www.mkyong.com/jdbc/how-to-connect-to-mysql-with-jdbc-driver-java/)

[parameter test](http://web.archive.org/web/20190228162831/http://www.mkyong.com/tag/parameter-test/) [testng](http://web.archive.org/web/20190228162831/http://www.mkyong.com/tag/testng/)







