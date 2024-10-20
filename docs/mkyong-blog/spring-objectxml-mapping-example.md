# Spring 对象/XML 映射示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/spring-objectxml-mapping-example/>

Spring 的对象/XML 映射将对象转换成 XML，反之亦然。这个过程也称为

1.  **XML 编组**–将对象转换为 XML。
2.  **XML 解组**–将 XML 转换成对象。

在本教程中，我们将向您展示如何使用 Spring 的 oxm 来进行转换，**对象< - Spring oxm - > XML** 。

**Note**
No nonsense, for why and what benefits of using Spring’s oxm, read this [official Spring Object/XML mapping](http://web.archive.org/web/20190223073326/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/oxm.html) article.

## 1.项目依赖性

本例中的依赖关系。

**Note**
Spring’s oxm itself doesn’t handle the XML marshalling or UnMarshalling, it depends developer to inject their prefer XML binding framework. In this case, you will use Castor binding framework.

```java
 <properties>
		<spring.version>3.0.5.RELEASE</spring.version>
	</properties>

	<dependencies>

		<!-- Spring 3 dependencies -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- spring oxm -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-oxm</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- Uses Castor for XML -->
		<dependency>
			<groupId>org.codehaus.castor</groupId>
			<artifactId>castor</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- Castor need this -->
		<dependency>
			<groupId>xerces</groupId>
			<artifactId>xercesImpl</artifactId>
			<version>2.8.1</version>
		</dependency>

	</dependencies> 
```

 ## 2.简单对象

一个简单的对象，后来把它转换成 XML 文件。

```java
 package com.mkyong.core.model;

public class Customer {

	String name;
	int age;
	boolean flag;
	String address;

	//standard getter, setter and toString() methods.
} 
```

 ## 3.编组器和解组器

这个类将通过 Spring 的 oxm 接口:`Marshaller`和`Unmarshaller`处理转换。

```java
 package com.mkyong.core;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import javax.xml.transform.stream.StreamResult;
import javax.xml.transform.stream.StreamSource;
import org.springframework.oxm.Marshaller;
import org.springframework.oxm.Unmarshaller;

public class XMLConverter {

	private Marshaller marshaller;
	private Unmarshaller unmarshaller;

	public Marshaller getMarshaller() {
		return marshaller;
	}

	public void setMarshaller(Marshaller marshaller) {
		this.marshaller = marshaller;
	}

	public Unmarshaller getUnmarshaller() {
		return unmarshaller;
	}

	public void setUnmarshaller(Unmarshaller unmarshaller) {
		this.unmarshaller = unmarshaller;
	}

	public void convertFromObjectToXML(Object object, String filepath)
		throws IOException {

		FileOutputStream os = null;
		try {
			os = new FileOutputStream(filepath);
			getMarshaller().marshal(object, new StreamResult(os));
		} finally {
			if (os != null) {
				os.close();
			}
		}
	}

	public Object convertFromXMLToObject(String xmlfile) throws IOException {

		FileInputStream is = null;
		try {
			is = new FileInputStream(xmlfile);
			return getUnmarshaller().unmarshal(new StreamSource(is));
		} finally {
			if (is != null) {
				is.close();
			}
		}
	}

} 
```

## 4.弹簧配置

在 Spring 的 bean 配置文件中，注入`CastorMarshaller`作为 XML 绑定框架。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="XMLConverter" class="com.mkyong.core.XMLConverter">
		<property name="marshaller" ref="castorMarshaller" />
		<property name="unmarshaller" ref="castorMarshaller" />
	</bean>
	<bean id="castorMarshaller" class="org.springframework.oxm.castor.CastorMarshaller" />

</beans> 
```

## 5.试验

运行它。

```java
 package com.mkyong.core;

import java.io.IOException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.mkyong.core.model.Customer;

public class App {
	private static final String XML_FILE_NAME = "customer.xml";

	public static void main(String[] args) throws IOException {
		ApplicationContext appContext = new ClassPathXmlApplicationContext("App.xml");
		XMLConverter converter = (XMLConverter) appContext.getBean("XMLConverter");

		Customer customer = new Customer();
		customer.setName("mkyong");
		customer.setAge(30);
		customer.setFlag(true);
		customer.setAddress("This is address");

		System.out.println("Convert Object to XML!");
		//from object to XML file
		converter.convertFromObjectToXML(customer, XML_FILE_NAME);
		System.out.println("Done \n");

		System.out.println("Convert XML back to Object!");
		//from XML to object
		Customer customer2 = (Customer)converter.convertFromXMLToObject(XML_FILE_NAME);
		System.out.println(customer2);
		System.out.println("Done");

	}
} 
```

输出

```java
 Convert Object to XML!
Done 

Convert XML back to Object!
Customer [name=mkyong, age=30, flag=true, address=This is address]
Done 
```

下面的 XML 文件" **customer.xml** 在您的项目根文件夹中生成。

*文件:customer.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<customer flag="true" age="30">
	<address>This is address</address>
	<name>mkyong</name>
</customer> 
```

## Castor XML 映射

等等，为什么旗帜和年龄转换成属性？这是一种控制哪个字段应该用作属性或元素的方法吗？当然，您可以使用 [Castor XML 映射](http://web.archive.org/web/20190223073326/http://www.castor.org/xml-mapping.html target=)来定义对象和 XML 之间的关系。

创建以下映射文件，并将其放入项目类路径中。

*文件:mapping.xml*

```java
 <mapping>
	<class name="com.mkyong.core.model.Customer">

		<map-to xml="customer" />

		<field name="age" type="integer">
			<bind-xml name="age" node="attribute" />
		</field>

		<field name="flag" type="boolean">
			<bind-xml name="flag" node="element" />
		</field>

		<field name="name" type="string">
			<bind-xml name="name" node="element" />
		</field>

		<field name="address" type="string">
			<bind-xml name="address" node="element" />
		</field>
	</class>
</mapping> 
```

在 Spring bean 配置文件中，通过“ **mappingLocation** 将上述 **mapping.xml** 注入到 CastorMarshaller 中。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="XMLConverter" class="com.mkyong.core.XMLConverter">
		<property name="marshaller" ref="castorMarshaller" />
		<property name="unmarshaller" ref="castorMarshaller" />
	</bean>
	<bean id="castorMarshaller" class="org.springframework.oxm.castor.CastorMarshaller" >
		<property name="mappingLocation" value="classpath:mapping.xml" />
	</bean>

</beans> 
```

再次测试，XML 文件“ **customer.xml** ”将被更新。

*文件:customer.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<customer age="30">
	<flag>true</flag>
	<name>mkyong</name>
	<address>This is address</address>
</customer> 
```

## 下载源代码

Download it – [Spring3-Object-XML-Mapping-Example.zip](http://web.archive.org/web/20190223073326/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-Object-XML-Mapping-Example.zip) (7 KB)

## 参考

1.  [Castor XML 映射](http://web.archive.org/web/20190223073326/http://www.castor.org/xml-mapping.html)
2.  [Spring 对象/XML 映射引用](http://web.archive.org/web/20190223073326/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/oxm.html)

[oxm](http://web.archive.org/web/20190223073326/http://www.mkyong.com/tag/oxm/) [spring3](http://web.archive.org/web/20190223073326/http://www.mkyong.com/tag/spring3/)







