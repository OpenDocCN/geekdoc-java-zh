# 如何在 Struts 2 中获得 ServletContext

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-get-the-servletcontext-in-struts-2/>

在 Struts 2 中，可以使用以下两种方法来获取 **ServletContext** 对象。

## 1.ServletActionContext

直接从**org . Apache . struts 2 . servletactioncontext**获取 ServletContext 对象。

```java
 import javax.servlet.ServletContext;
import org.apache.struts2.ServletActionContext;
import com.opensymphony.xwork2.ActionSupport;

public class CustomerAction extends ActionSupport{

	public String execute() throws Exception {

		ServletContext context = ServletActionContext.getServletContext();

		return SUCCESS;

	}

} 
```

## 2.ServletContextAware

使您的类实现**org . Apache . struts 2 . util . servletcontextaware**接口。

When Struts 2 ‘**servlet-config**’ interceptor is seeing that an Action class is implemented the **ServletContextAware** interface, it will pass a **ServletContext** reference to the requested Action class via the **setServletContext()** method.

```java
 import javax.servlet.ServletContext;
import org.apache.struts2.util.ServletContextAware;
import com.opensymphony.xwork2.ActionSupport;

public class CustomerAction 
    extends ActionSupport implements ServletContextAware{

	ServletContext context;

	public String execute() throws Exception {

		return SUCCESS;

	}

	public void setServletContext(ServletContext context) {
		this.context = context;
	}
} 
```

## 参考

1.  [Struts 2 ServletContextAware 文档](http://web.archive.org/web/20220316201916/https://struts.apache.org/2.0.11.1/struts2-core/apidocs/org/apache/struts2/util/ServletContextAware.html)

<input type="hidden" id="mkyong-current-postId" value="6282">