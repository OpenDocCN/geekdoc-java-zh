# 如何处理 Struts 中的 404 错误

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/how-to-handle-404-error-in-struts/>

“ **HTTP Status 404** ”错误代码表示系统找不到您请求的页面或资源。实际上，您应该显示自定义的 404 错误页面。但是，它是在 **web.xml** 中处理的，而不是在 Struts 框架中。举个例子，

```java
 <web-app>
  ...
  <error-page>
    <error-code>404</error-code>
    <location>/pages/error404.jsp</location>
  </error-page>

</web-app> 
```

当系统遇到 404 错误时，它会转发到您自定义的 404 错误页面" **/pages/error404.jsp** "。

**struts-config.xml**

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Maven Struts Examples</display-name>

  <servlet>
    <servlet-name>action</servlet-name>
    <servlet-class>
        org.apache.struts.action.ActionServlet
    </servlet-class>
    <init-param>
        <param-name>config</param-name>
        <param-value>
         /WEB-INF/struts-config.xml
        </param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
       <servlet-name>action</servlet-name>
       <url-pattern>*.do</url-pattern>
  </servlet-mapping>

  <error-page>
    <error-code>404</error-code>
    <location>/pages/error404.jsp</location>
  </error-page>

</web-app> 
```

[404](http://web.archive.org/web/20190309050027/http://www.mkyong.com/tag/404/) [struts](http://web.archive.org/web/20190309050027/http://www.mkyong.com/tag/struts/)![](img/8b4a5939603194215d333e7b51893cf7.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190309050027/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4645">







