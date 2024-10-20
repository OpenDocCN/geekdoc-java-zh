# 带有 getResource()示例的 Spring 资源加载器

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-resource-loader-with-getresource-example/>

Spring 的资源加载器提供了一个非常通用的 **getResource()** 方法来从文件系统、类路径或 URL 获取资源，比如(文本文件、媒体文件、图像文件……)。您可以从应用程序上下文中获取 **getResource()** 方法。

下面的例子展示了如何使用 **getResource()** 从

**1。文件系统**

```java
 Resource resource = appContext.getResource("file:c:\\testing.txt"); 
```

**2。URL 路径**

```java
 Resource resource = 
          appContext.getResource("url:http://www.yourdomain.com/testing.txt"); 
```

**3。类别路径**

```java
 Resource resource = 
          appContext.getResource("classpath:com/mkyong/common/testing.txt"); 
```

您只需要指定资源位置，Spring 会处理剩下的工作并返回一个资源对象。

使用`getResource()`方法的完整示例。

```java
 package com.mkyong.common;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.Resource;
public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext appContext = 
    	   new ClassPathXmlApplicationContext(new String[] {"If-you-have-any.xml"});

    	Resource resource = 
           appContext.getResource("classpath:com/mkyong/common/testing.txt");

    try{
     	  InputStream is = resource.getInputStream();
          BufferedReader br = new BufferedReader(new InputStreamReader(is));

          String line;
          while ((line = br.readLine()) != null) {
             System.out.println(line);
       	  } 
          br.close();

    	}catch(IOException e){
    		e.printStackTrace();
    	}

    }
} 
```

## Bean 资源加载器(ResourceLoaderAware)

既然 bean 没有应用程序上下文访问权限，那么 bean 如何访问资源呢？解决方法是实现**resource loader ware**接口，并为 **ResourceLoader** 对象创建 setter 方法。Spring 会将资源加载器插入到 bean 中。

```java
 package com.mkyong.customer.services;

import org.springframework.context.ResourceLoaderAware;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;

public class CustomerService implements ResourceLoaderAware
{
	private ResourceLoader resourceLoader;

	public void setResourceLoader(ResourceLoader resourceLoader) {
		this.resourceLoader = resourceLoader;
	}

	public Resource getResource(String location){
		return resourceLoader.getResource(location);
	}
} 
```

Bean 配置文件

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

   <bean id="customerService" 
           class="com.mkyong.customer.services.CustomerService" />

</beans> 
```

运行它

```java
 package com.mkyong.common;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.Resource;

import com.mkyong.customer.services.CustomerService;
public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext appContext = 
    	   new ClassPathXmlApplicationContext(new String[] {"Spring-Customer.xml"});

    	CustomerService cust = 
           (CustomerService)appContext.getBean("customerService");

    	Resource resource = 
            cust.getResource("classpath:com/mkyong/common/testing.txt");

    try{
          InputStream is = resource.getInputStream();
          BufferedReader br = new BufferedReader(new InputStreamReader(is));

          String line;
          while ((line = br.readLine()) != null) {
     	       System.out.println(line);
          } 
          br.close();

    	}catch(IOException e){
    		e.printStackTrace();
    	}

    }
} 
```

现在，您可以从 bean 中获取资源。

 ## 结论

如果没有这个 getResource()方法，您将需要用不同的解决方案处理不同的资源，比如文件系统资源的文件对象，URL 资源的 URL 对象。Spring 确实用这个超级通用的 **getResource()** 方法做得很好，它确实节省了我们处理资源的时间。

 ## 下载源代码

Download it – [Spring-getResource-Example.zip](http://web.archive.org/web/20190308030421/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-getResource-Example.zip)[spring](http://web.archive.org/web/20190308030421/http://www.mkyong.com/tag/spring/)







