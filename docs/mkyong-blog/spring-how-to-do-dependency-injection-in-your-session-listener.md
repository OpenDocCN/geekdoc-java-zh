# spring——如何在会话监听器中进行依赖注入

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-how-to-do-dependency-injection-in-your-session-listener/>

Spring 附带了一个" **ContextLoaderListener** "监听器，用于将 Spring 依赖注入会话监听器。在本教程中，它通过在会话监听器中添加一个 Spring 依赖注入 bean 来修改这个 [HttpSessionListener 示例](http://web.archive.org/web/20190224160129/http://www.mkyong.com/servlet/a-simple-httpsessionlistener-example-active-sessions-counter/)。

## 1.春豆

创建一个简单的计数器服务来打印创建的会话总数。

*文件:CounterService.java*

```java
 package com.mkyong.common;

public class CounterService{

	public void printCounter(int count){
		System.out.println("Total session created : " + count);
	}

} 
```

*文件:counter . XML*–Bean 配置文件。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="counterService" class="com.mkyong.common.CounterService" />

</beans> 
```

 ## 2.WebApplicationContextUtils

使用“`WebApplicationContextUtils`”来获取 Spring 的上下文，然后您可以用正常的 Spring 方式获取任何声明的 Spring 的 bean。

*文件:SessionCounterListener.java*

```java
 package com.mkyong.common;

import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;
import org.springframework.context.ApplicationContext;
import org.springframework.web.context.support.WebApplicationContextUtils;

public class SessionCounterListener implements HttpSessionListener {

     private static int totalActiveSessions;

     public static int getTotalActiveSession(){
           return totalActiveSessions;
     }

    @Override
    public void sessionCreated(HttpSessionEvent arg0) {
           totalActiveSessions++;
           System.out.println("sessionCreated - add one session into counter");	
           printCounter(arg0);
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent arg0) {
           totalActiveSessions--;
           System.out.println("sessionDestroyed - deduct one session from counter");	
           printCounter(arg0);
    }	

    private void printCounter(HttpSessionEvent sessionEvent){

          HttpSession session = sessionEvent.getSession();

          ApplicationContext ctx = 
                WebApplicationContextUtils.
                      getWebApplicationContext(session.getServletContext());

          CounterService counterService = 
                      (CounterService) ctx.getBean("counterService");

          counterService.printCounter(totalActiveSessions);
    }
} 
```

 ## 3.综合

唯一的问题是，您的 web 应用程序如何知道在哪里加载 Spring bean 配置文件？秘密就在“web.xml”文件中。

1.  将“`ContextLoaderListener`”注册为第一个监听器，让您的 web 应用程序知道 Spring context loader。
2.  配置“`contextConfigLocation`”并定义 Spring 的 bean 配置文件。

*文件:web.xml*

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>

  <context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>/WEB-INF/Spring/counter.xml</param-value>
  </context-param>

  <listener>
        <listener-class>
            org.springframework.web.context.ContextLoaderListener
        </listener-class>
  </listener>

  <listener>
	<listener-class>
            com.mkyong.common.SessionCounterListener
        </listener-class>
  </listener>

  <servlet>
	<servlet-name>Spring DI Servlet Listener</servlet-name>
	<servlet-class>com.mkyong.common.App</servlet-class>
  </servlet>

  <servlet-mapping>
	<servlet-name>Spring DI Servlet Listener</servlet-name>
	<url-pattern>/Demo</url-pattern>
  </servlet-mapping>

</web-app> 
```

*文件:App.java*

```java
 package com.mkyong.common;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class App extends HttpServlet{

  public void doGet(HttpServletRequest request, HttpServletResponse response)
        throws IOException{

        HttpSession session = request.getSession(); //sessionCreated() is executed
        session.setAttribute("url", "mkyong.com"); 
        session.invalidate();  //sessionDestroyed() is executed

        PrintWriter out = response.getWriter();
        out.println("<html>");
        out.println("<body>");
        out.println("<h1>Spring Dependency Injection into Servlet Listenner</h1>");
        out.println("</body>");
        out.println("</html>");	

   }
} 
```

启动 Tomcat，访问网址“*http://localhost:8080/spring web example/Demo*”。

*输出*

```java
 sessionCreated - add one session into counter
Total session created : 1
sessionDestroyed - deduct one session from counter
Total session created : 0 
```

查看控制台输出，通过 Spring DI 获得计数器服务 bean，并打印会话总数。

## 结论

在 Spring 中，“`ContextLoaderListener`”是将 Spring 依赖注入集成到几乎所有 web 应用程序的一种通用方式。

## [计] 下载

Download It – [SpringWeb-DI-Session-listener-Example.zip](http://web.archive.org/web/20190224160129/http://www.mkyong.com/wp-content/uploads/2010/03/SpringWeb-DI-Session-listener-Example.zip)[session listener](http://web.archive.org/web/20190224160129/http://www.mkyong.com/tag/session-listener/) [spring](http://web.archive.org/web/20190224160129/http://www.mkyong.com/tag/spring/)







