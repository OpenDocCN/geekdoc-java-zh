# 如何在 Struts 2 中获得 HttpServletResponse

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-get-the-httpservletresponse-in-struts-2/>

在 Struts 2 中，您可以使用以下两种方法来获取`HttpServletResponse`对象。

## 1.ServletActionContext

通过`ServletActionContext`类访问`HttpServletResponse`。

```java
 package com.mkyong.common.action;

import javax.servlet.http.HttpServletResponse;
import org.apache.struts2.ServletActionContext;

public class LocaleAction{
	//business logic
	public String execute() {
		HttpServletResponse response = ServletActionContext.getResponse();

		return "SUCCESS";
	}
} 
```

 ## 2.ServletResponseAware

通过实现`ServletResponseAware`接口来访问`HttpServletResponse`，并覆盖 `setServletResponse()`方法。

**Note**
When Struts 2 ‘**servlet-config**‘ interceptor is seeing that an Action class is implemented the `ServletResponseAware` interface, it will pass a `HttpServletResponse` reference to the requested Action class via the `setServletResponse()` menthod.

当然，您可以创建一个自定义的`getServletResponse()`来轻松获得`HttpServletResponse`。

```java
 package com.mkyong.common.action;

import java.util.Locale;
import javax.servlet.http.HttpServletResponse;
import org.apache.struts2.interceptor.ServletResponseAware;

public class LocaleAction implements ServletResponseAware{

	HttpServletResponse response;

	//business logic
	public String execute() {
		Locale locale = getServletResponse().getLocale();
		return "SUCCESS";
	}

	public void setServletResponse(HttpServletResponse response) {
		this.response = response;
	}
	public HttpServletResponse getServletResponse() {
		return this.response;
	}	
} 
```

两种机制都获得相同的`HttpServletResponse`对象，但是 Struts 2 文档推荐使用`ServletResponseAware`，参见下面的参考资料:

 ## 参考

1.  [http://struts . Apache . org/2 . x/docs/how-can-we-access-the-http servlet response . html](http://web.archive.org/web/20190304032352/http://struts.apache.org/2.x/docs/how-can-we-access-the-httpservletresponse.html)
2.  [http://struts . Apache . org/2 . 1 . 2/struts 2-core/API docs/org/Apache/struts 2/interceptor/servletresponseaware . html](http://web.archive.org/web/20190304032352/http://struts.apache.org/2.1.2/struts2-core/apidocs/org/apache/struts2/interceptor/ServletResponseAware.html)

[httpservletresponse](http://web.archive.org/web/20190304032352/http://www.mkyong.com/tag/httpservletresponse/) [struts2](http://web.archive.org/web/20190304032352/http://www.mkyong.com/tag/struts2/)







