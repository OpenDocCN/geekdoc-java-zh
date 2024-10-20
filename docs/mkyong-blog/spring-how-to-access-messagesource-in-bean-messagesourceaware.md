# spring——如何在 bean 中访问 message source(message source aware)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-how-to-access-messagesource-in-bean-messagesourceaware/>

在上一个教程中，您可以通过 ApplicationContext 获得[消息源。但是对于获取 MessageSource 的 bean，您必须实现 **MessageSourceAware** 接口。](http://web.archive.org/web/20190213133419/http://www.mkyong.com/spring/spring-resource-bundle-with-resourcebundlemessagesource-example/)

## 例子

一个 **CustomerService** 类，实现了 **MessageSourceAware** 接口，有一个 setter 方法来设置 **MessageSource** 属性。

During Spring container initialization, if any class which implements the **MessageSourceAware** interface, Spring will automatically inject the MessageSource into the class via **setMessageSource(MessageSource messageSource)** setter method.

```java
 package com.mkyong.customer.services;

import java.util.Locale;

import org.springframework.context.MessageSource;
import org.springframework.context.MessageSourceAware;

public class CustomerService implements MessageSourceAware
{
	private MessageSource messageSource;

	public void setMessageSource(MessageSource messageSource) {
		this.messageSource = messageSource;
	}

	public void printMessage(){
		String name = messageSource.getMessage("customer.name", 
    			new Object[] { 28, "http://www.mkyong.com" }, Locale.US);

    	System.out.println("Customer name (English) : " + name);

    	String namechinese = messageSource.getMessage("customer.name", 
    			new Object[] { 28, "http://www.mkyong.com" }, 
                        Locale.SIMPLIFIED_CHINESE);

    	System.out.println("Customer name (Chinese) : " + namechinese);
	}

} 
```

运行它

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
    		new ClassPathXmlApplicationContext(
			    new String[] {"locale.xml","Spring-Customer.xml"});

    	CustomerService cust = (CustomerService)context.getBean("customerService");
    	cust.printMessage();
    }
} 
```

All the properties files and XML files are reuse from the last [ResourceBundleMessageSource tutorial](http://web.archive.org/web/20190213133419/http://www.mkyong.com/spring/spring-resource-bundle-with-resourcebundlemessagesource-example/).Download it – [Spring-MessageSource-Example.zip](http://web.archive.org/web/20190213133419/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-MessageSource-Example.zip)[spring](http://web.archive.org/web/20190213133419/http://www.mkyong.com/tag/spring/)![](img/6fcde1f7f378cc4af4be1a89998a0d0e.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190213133419/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3940">







