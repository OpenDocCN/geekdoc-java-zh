# Spring init 方法和 destroy 方法示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-init-method-and-destroy-method-example/>

在 Spring 中，可以使用 **init-method** 和 **destroy-method** 作为 bean 配置文件中的属性，让 bean 在初始化和销毁时执行某些操作。替代[初始化 Bean 和可处置 Bean 接口](http://web.archive.org/web/20190310101557/http://www.mkyong.com/spring/spring-initializingbean-and-disposablebean-example/)。

## 例子

这里有一个例子向你展示如何使用**初始化方法**和**销毁方法**。

```java
 package com.mkyong.customer.services;

public class CustomerService
{
	String message;

	public String getMessage() {
	  return message;
	}

	public void setMessage(String message) {
	  this.message = message;
	}

	public void initIt() throws Exception {
	  System.out.println("Init method after properties are set : " + message);
	}

	public void cleanUp() throws Exception {
	  System.out.println("Spring Container is destroy! Customer clean up");
	}

} 
```

*文件:Spring-Customer.xml* ，定义 bean 中的 **init-method** 和 **destroy-method** 属性。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerService" class="com.mkyong.customer.services.CustomerService" 
		init-method="initIt" destroy-method="cleanUp">

		<property name="message" value="i'm property message" />
	</bean>

</beans> 
```

运行它

```java
 package com.mkyong.common;

import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.mkyong.customer.services.CustomerService;

public class App 
{
    public static void main( String[] args )
    {
    	ConfigurableApplicationContext context = 
		new ClassPathXmlApplicationContext(new String[] {"Spring-Customer.xml"});

    	CustomerService cust = (CustomerService)context.getBean("customerService");

    	System.out.println(cust);

    	context.close();
    }
} 
```

configurableapplicationcontext . close 将关闭应用程序上下文，释放所有资源并销毁所有缓存的单体 beans。
 *输出*

```java
 Init method after properties are set : i'm property message
com.mkyong.customer.services.CustomerService@47393f
...
INFO: Destroying singletons in org.springframework.beans.factory.
support.DefaultListableBeanFactory@77158a: 
defining beans [customerService]; root of factory hierarchy
Spring Container is destroy! Customer clean up 
```

设置 message 属性后调用 **initIt()** 方法，在 context.close()后调用 **cleanUp()** 方法；

**Thoughts…**
It’s always recommended to use **init-method** and **destroy-method** in bean configuration file, instead of implement the InitializingBean and DisposableBean interface to cause unnecessarily coupled your code to Spring. ## 下载源代码

Download It – [Spring-init-method-destroy-method-Example.zip](http://web.archive.org/web/20190310101557/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-init-method-destroy-method-Example.zip)[spring](http://web.archive.org/web/20190310101557/http://www.mkyong.com/tag/spring/)![](img/3c521ee774b68fe1ca3181dfaeb46466.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190310101557/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3896">







