# ModelAndView 的模型值不会通过 el 显示在 JSP 中

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/modelandviews-model-value-is-not-displayed-in-jsp-via-el/>

## 问题

在 Spring MVC 开发中，开发人员试图在模型中设置一个值，并通过 el 在 JSP 中显示该值，例如 **${msg}** ，但它只是按原样输出结果——**$ { msg }**，而不是存储在模型中的“值”。EL 在 JSP 中就是不工作，为什么？

**弹簧控制器**

```java
 import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.AbstractController;

public class ABCController extends AbstractController{

	@Override
	protected ModelAndView handleRequestInternal(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		ModelAndView model = new ModelAndView("HelloWorldPage");
		model.addObject("msg", "hello world");

		return model;
	}

} 
```

**JSP 页面**

```java
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<body>
             ${msg}
</body>
</html> 
```

## 解决办法

这是大多数 Spring MVC hello world 示例中常见的问题。实际上这是由旧的 JSP 1.2 描述符引起的。

## 1.JSP 1.2

如果你使用的是由 DTD 定义的**旧 JSP 1.2 描述符，例如
web . XML**

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
//...
</web-app> 
```

默认情况下，EL 被禁用或忽略，您必须手动启用它，以便它将输出存储在“msg”模型中的值。

```java
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
<%@ page isELIgnored="false" %>
</head>
<body>
           ${msg}
</body>
</html> 
```

## 2.JSP 2.0

如果你使用的是由 w3c schema 定义的**标准 JSP 2.0 描述符，例如
**web.xml****

```java
 <web-app id="WebApp_ID" version="2.4" 

	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
	http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
//...
</web-app> 
```

EL 是默认启用的，您应该看到存储在“msg”模型中的值，即“hello world”。

## 参考

1.  [编写使用指令](http://web.archive.org/web/20200313190244/http://java.boot.by/wcd-guide/ch06s02.html)的 JSP 代码

[el](http://web.archive.org/web/20200313190244/https://mkyong.com/tag/el/) [jsp](http://web.archive.org/web/20200313190244/https://mkyong.com/tag/jsp/) [spring mvc](http://web.archive.org/web/20200313190244/https://mkyong.com/tag/spring-mvc/)<input type="hidden" id="mkyong-current-postId" value="6434">