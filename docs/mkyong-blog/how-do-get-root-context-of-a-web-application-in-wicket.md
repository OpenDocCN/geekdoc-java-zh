# 如何在 Wicket 中获取 web 应用程序的根上下文

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-do-get-root-context-of-a-web-application-in-wicket/>

在 Wicket 中，您可以使用下面的方法来获取项目的"**根上下文**。

```java
 WebApplication.get().getServletContext().getServletContextName() 
```

举个例子，

```java
 public static String getRootContext(){

		String rootContext = "";

		WebApplication webApplication = WebApplication.get();
		if(webApplication!=null){
			ServletContext servletContext = webApplication.getServletContext();
			if(servletContext!=null){
				rootContext = servletContext.getServletContextName();
			}else{
				//do nothing
			}
		}else{
			//do nothing
		}

		return rootContext;

} 
```

[context](http://web.archive.org/web/20190114230103/http://www.mkyong.com/tag/context/) [wicket](http://web.archive.org/web/20190114230103/http://www.mkyong.com/tag/wicket/)![](img/89a85db97c43aff58779e8a909e4878f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190114230103/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1436">







