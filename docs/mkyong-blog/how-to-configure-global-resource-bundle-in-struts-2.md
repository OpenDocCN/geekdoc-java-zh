# 如何在 Struts 2 中配置全局资源包

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-configure-global-resource-bundle-in-struts-2/>

通常，您可能需要一个全局资源包(属性文件)来存储应用程序中所有类都可以使用的消息。

Download It – [Struts2-global-resource-bundle-Example.zip](http://web.archive.org/web/20190302180352/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-global-resource-bundle-Example.zip)

在 Struts 2 中，有三种方式来配置全局资源包:

## 1.struts.properties

在“struts.properties”文件中配置全局资源包，这里定义了一个名为“ **global.properties** 的属性文件作为全局资源包。

```java
 struts.custom.i18n.resources = global 
```

对于多个资源包，只需用逗号分隔属性文件。

```java
 struts.custom.i18n.resources = global, another-properties-file 
```

 ## 2.struts.xml

或者，您可以在 **struts.xml** 配置文件中将全局资源包配置为一个常量值。

```java
 <struts>
   <constant name="struts.custom.i18n.resources" value="global" /> 	
</struts> 
```

 ## 3.听众

最后一种方法是使用 servlet 监听器加载一个属性文件作为全局资源包。

```java
 package com.mkyong.common.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

import com.opensymphony.xwork2.util.LocalizedTextUtil;

public class GlobalMessagesListener implements ServletContextListener {

	  private static final String DEFAULT_RESOURCE = "global";

	  public void contextInitialized(ServletContextEvent arg0) {
	    LocalizedTextUtil.addDefaultResourceBundle(DEFAULT_RESOURCE);
	  }

	  public void contextDestroyed(ServletContextEvent arg0) {
	  }
} 
```

**web.xml**

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Struts 2 Web Application</display-name>

  <filter>
	<filter-name>struts2</filter-name>
	<filter-class>org.apache.struts2.dispatcher.FilterDispatcher</filter-class>
  </filter>

  <filter-mapping>
	<filter-name>struts2</filter-name>
	<url-pattern>/*</url-pattern>
  </filter-mapping>

  <listener>
  	<listener-class>
          com.mkyong.common.listener.GlobalMessagesListener
        </listener-class>
  </listener>

</web-app> 
```

[resource bundle](http://web.archive.org/web/20190302180352/http://www.mkyong.com/tag/resource-bundle/) [struts2](http://web.archive.org/web/20190302180352/http://www.mkyong.com/tag/struts2/)







