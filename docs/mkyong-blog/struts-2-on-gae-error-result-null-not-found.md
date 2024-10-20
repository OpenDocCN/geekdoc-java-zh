# GAE 上的 struts 2–错误:未找到结果“null”

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/struts-2-on-gae-error-result-null-not-found/>

## 问题

在以下环境中，在 Google App Engine 上开发 Struts2。

1.  struts 2.3.1.2
2.  JDK 1.6
3.  Eclipse 3.7+Eclipse 的 Google 插件
4.  谷歌应用引擎 Java SDK 1.6.3.1

刚开始一个简单的 Struts2 hello world 例子，当访问 action 类时，在本地开发和实际生产的 GAE 环境中都出现错误“Error: result 'null' not found”。

![struts2 on gae - result null error](img/874b576bd66f95a2185194a2f52bca5a.png "struts2-gae-result-null-error") ## 解决办法

OGNL 正在执行一些安全检查，这在 GAE 是不支持的。为了让 **Struts 2 在 GAE 环境**中工作，您需要在`web.xml`中创建一个监听器，并将 OGNL 安全管理器设置为空。

```java
 OgnlRuntime.setSecurityManager(null); 
```

完整的例子。

```java
 package com.mkyong.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.http.HttpSessionAttributeListener;
import javax.servlet.http.HttpSessionBindingEvent;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

import ognl.OgnlRuntime;

public class Struts2ListenerOnGAE implements ServletContextListener,
		HttpSessionListener, HttpSessionAttributeListener {

	public void contextInitialized(ServletContextEvent sce) {
		OgnlRuntime.setSecurityManager(null);
	}

	@Override
	public void contextDestroyed(ServletContextEvent arg0) {
		// TODO Auto-generated method stub

	}

	@Override
	public void sessionCreated(HttpSessionEvent arg0) {
		// TODO Auto-generated method stub

	}

	@Override
	public void sessionDestroyed(HttpSessionEvent arg0) {
		// TODO Auto-generated method stub

	}

	@Override
	public void attributeAdded(HttpSessionBindingEvent arg0) {
		// TODO Auto-generated method stub

	}

	@Override
	public void attributeRemoved(HttpSessionBindingEvent arg0) {
		// TODO Auto-generated method stub

	}

	@Override
	public void attributeReplaced(HttpSessionBindingEvent arg0) {
		// TODO Auto-generated method stub

	}

} 
```

*文件:web.xml*

```java
 <?xml version="1.0" encoding="utf-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

        xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" version="2.5">

	<filter>
	   <filter-name>struts2</filter-name>
	   <filter-class>
             org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
           </filter-class>
	</filter>

	<filter-mapping>
		<filter-name>struts2</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<listener>
		<listener-class>com.mkyong.listener.Struts2ListenerOnGAE</listener-class>
	</listener>

</web-app> 
```

 ## 参考

1.  [在 GAE 部署 Struts 2 时出现的问题](http://web.archive.org/web/20190226094949/http://struts.apache.org/2.2.1/docs/google-app-engine-gae.html)

[gae](http://web.archive.org/web/20190226094949/http://www.mkyong.com/tag/gae/) [null](http://web.archive.org/web/20190226094949/http://www.mkyong.com/tag/null/) [struts2](http://web.archive.org/web/20190226094949/http://www.mkyong.com/tag/struts2/)







