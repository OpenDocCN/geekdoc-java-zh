# Spring MVC MultiActionController 注释示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-annotation-example/>

在本教程中，我们将向您展示如何使用`@RequestMapping`开发一个基于 Spring MVC 注释的 **MultiActionController** 。

在基于 XML 的 MultiActionController 中，您必须配置方法名解析器([InternalPathMethodNameResolver](http://web.archive.org/web/20190214221647/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/)、[propertiesmethodname resolver](http://web.archive.org/web/20190214221647/http://www.mkyong.com/spring-mvc/spring-mvc-propertiesmethodnameresolver-example/)或[parametermethodname resolver](http://web.archive.org/web/20190214221647/http://www.mkyong.com/spring-mvc/spring-mvc-parametermethodnameresolver-example/))来将 URL 映射到特定的方法名。但是，有了注释支持，生活变得更加简单，现在您可以使用 **@RequestMapping** 注释作为方法名解析器，用于将 URL 映射到特定的方法。

**Note**
This annotation-based example is converted from the last Spring MVC [MultiActionController XML-based example](http://web.archive.org/web/20190214221647/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/). So, please compare and spots the different.

要配置它，在方法名上方定义映射 URL 的 **@RequestMapping** 。

```java
 package com.mkyong.common.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class CustomerController{

	@RequestMapping("/customer/add.htm")
	public ModelAndView add(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerAddView");

	}

	@RequestMapping("/customer/delete.htm")
	public ModelAndView delete(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerDeleteView");

	}

	@RequestMapping("/customer/update.htm")
	public ModelAndView update(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerUpdateView");

	}

	@RequestMapping("/customer/list.htm")
	public ModelAndView list(HttpServletRequest request,
		HttpServletResponse response) throws Exception {

		return new ModelAndView("CustomerListView");

	}
} 
```

现在，URL 将按照以下模式映射到方法名:

1.  /customer/add . htm –> add()方法
2.  /customer/delete . htm –> delete()方法
3.  /customer/update . htm –> update()方法
4.  /customer/list . htm –> list()方法

**Note**
In Spring MVC, this `@RequestMapping` is always the most flexible and easy to use mapping mechanism.

## 下载源代码

Download it – [SpringMVC-MultiActionController-Annotation-Example.zip](http://web.archive.org/web/20190214221647/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-MultiActionController-Annotation-Example.zip) (7KB)[spring mvc](http://web.archive.org/web/20190214221647/http://www.mkyong.com/tag/spring-mvc/)![](img/03a873bc4836c7f45c1821cca9b57e42.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214221647/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6771">







