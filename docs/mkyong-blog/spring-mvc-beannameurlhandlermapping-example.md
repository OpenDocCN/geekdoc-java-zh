# spring MVC BeanNameUrlHandlerMapping 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-beannameurlhandlermapping-example/>

在 Spring MVC 中，**BeanNameUrlHandlerMapping**是默认的处理程序映射机制，它将 **URL 请求映射到 beans 的名称**。举个例子，

```java
 <beans ...>

   <bean 
	class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>

   <bean name="/welcome.htm" 
        class="com.mkyong.common.controller.WelcomeController" />

   <bean name="/streetName.htm" 
        class="com.mkyong.common.controller.StreetNameController" />

   <bean name="/process*.htm" 
        class="com.mkyong.common.controller.ProcessController" />

</beans> 
```

在上面的例子中，如果 URI 模式

1.  **/welcome.htm** 被请求，DispatcherServlet 会将请求转发给“`WelcomeController`”。
2.  **/streetName.htm** 被请求，DispatcherServlet 将把请求转发给“`StreetNameController`”。
3.  **/processCreditCard.htm** 或 **/process{any thing}。htm** 被请求，DispatcherServlet 将把请求转发给“`ProcessController`”。

**Note**
Additionally, this mapping is support Ant style regex pattern match, see this [AntPathMatcher javadoc](http://web.archive.org/web/20190218213232/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/util/AntPathMatcher.html) for details.

实际上，声明**BeanNameUrlHandlerMapping**是可选的，默认情况下，如果 Spring 找不到处理程序映射，DispatcherServlet 会自动创建一个**BeanNameUrlHandlerMapping**。

因此，上面的 web.xml 文件等同于下面的 web.xml:

```java
 <beans ...>

   <bean name="/welcome.htm" 
            class="com.mkyong.common.controller.WelcomeController" />

   <bean name="/streetName.htm" 
            class="com.mkyong.common.controller.StreetNameController" />

   <bean name="/process*.htm" 
            class="com.mkyong.common.controller.ProcessController" />

</beans> 
```

## 下载源代码

Download it – [SpringMVC-BeanNameUrlHandlerMapping-Example.zip](http://web.archive.org/web/20190218213232/http://www.mkyong.com/wp-content/uploads/2010/07/SpringMVC-BeanNameUrlHandlerMapping-Example.zip) (7 KB) ## 参考

1.  [beannameurlhandermapping javadoc](http://web.archive.org/web/20190218213232/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/handler/BeanNameUrlHandlerMapping.html)
2.  [AntPathMatcher javadoc](http://web.archive.org/web/20190218213232/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/util/AntPathMatcher.html)

[spring mvc](http://web.archive.org/web/20190218213232/http://www.mkyong.com/tag/spring-mvc/) [url mapping](http://web.archive.org/web/20190218213232/http://www.mkyong.com/tag/url-mapping/)![](img/228777669241ce869d5fb68af6a26cce.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190218213232/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6453">







