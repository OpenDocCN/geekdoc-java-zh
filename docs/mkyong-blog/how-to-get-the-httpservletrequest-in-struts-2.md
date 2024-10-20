# 如何在 Struts 2 中获得 HttpServletRequest

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-get-the-httpservletrequest-in-struts-2/>

在 Struts 2 中，可以使用以下两种方法来获取 **HttpServletRequest** 对象。

## 1.ServletActionContext

直接从**org . Apache . struts 2 . servletactioncontext**获取 HttpServletRequest 对象。

```java
 import javax.servlet.http.HttpServletRequest;
import org.apache.struts2.ServletActionContext;

public class LocaleAction{
	//business logic
	public String execute() {
		HttpServletRequest request = ServletActionContext.getRequest();
		return "SUCCESS";
	}
} 
```

 ## 2.ServletRequestAware

使您的类实现了**org . Apache . struts 2 . interceptor . servletrequestaware**。

When Struts 2 ‘**servlet-config**‘ interceptor is seeing that an Action class is implemented the **ServletRequestAware** interface, it will pass a **HttpServletRequest** reference to the requested Action class via the **setServletRequest()** method.

```java
 import javax.servlet.http.HttpServletRequest;
import org.apache.struts2.interceptor.ServletRequestAware;

public class LocaleAction implements ServletRequestAware{

	HttpServletRequest request;

	//business logic
	public String execute() {
		String param = getServletRequest().getParameter("param");
		return "SUCCESS";

	}

	public void setServletRequest(HttpServletRequest request) {
		this.request = request;
	}

	public HttpServletRequest getServletRequest() {
		return this.request;
	}
} 
```

Struts 2 documentation is recommended **ServletRequestAware** instead of **ServletActionContext**. ## 参考

1.  [http://struts . Apache . org/2 . x/docs/how-can-we-access-the-http servlet request . html](http://web.archive.org/web/20190304031154/http://struts.apache.org/2.x/docs/how-can-we-access-the-httpservletrequest.html)
2.  [http://struts . Apache . org/2 . 0 . 6/struts 2-core/API docs/org/Apache/struts 2/interceptor/servletrequestaware . html](http://web.archive.org/web/20190304031154/http://struts.apache.org/2.0.6/struts2-core/apidocs/org/apache/struts2/interceptor/ServletRequestAware.html)

[httpservletrequest](http://web.archive.org/web/20190304031154/http://www.mkyong.com/tag/httpservletrequest/) [struts2](http://web.archive.org/web/20190304031154/http://www.mkyong.com/tag/struts2/)







