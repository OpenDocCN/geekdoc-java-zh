# javax . naming . namenotfoundexception:名称 jdbc 未在此上下文中绑定

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/javax-naming-namenotfoundexception-name-jdbc-is-not-bound-in-this-context/>

## 问题

JSF 2.0 web 应用程序，一个托管 bean 使用`@Resource`将数据源`jdbc/mkyongdb`注入到一个 ds 属性中。

```java
 @ManagedBean(name="customer")
@SessionScoped
public class CustomerBean implements Serializable{

	//resource injection
	@Resource(name="jdbc/mkyongdb")
	private DataSource ds; 
```

当部署到 Tomcat 6 时，它会遇到以下 MySQL 数据源配置的错误消息。

```java
 com.sun.faces.mgbean.ManagedBeanCreationException: 
        An error occurred performing resource injection on managed bean customer
	at com.sun.faces.mgbean.BeanBuilder.injectResources(BeanBuilder.java:207)
Caused by: com.sun.faces.spi.InjectionProviderException: 
        javax.naming.NameNotFoundException: Name jdbc is not bound in this Context
	at com.sun.faces.vendor.Tomcat6InjectionProvider.inject(Tomcat6InjectionProvider.java:84)
	at com.sun.faces.mgbean.BeanBuilder.injectResources(BeanBuilder.java:201)
	... 53 more
Caused by: javax.naming.NameNotFoundException: Name jdbc is not bound in this Context
	at org.apache.naming.NamingContext.lookup(NamingContext.java:770)
	at org.apache.naming.NamingContext.lookup(NamingContext.java:153)
	... 54 more 
```

 ## 解决办法

“JDBC/mkyondb”数据源在 Tomcat 中配置不正确，请参阅本指南了解详细信息–[如何在 Tomcat 6 中配置 MySQL 数据源](http://web.archive.org/web/20190224205626/http://www.mkyong.com/tomcat/how-to-configure-mysql-datasource-in-tomcat-6/)

[jdbc](http://web.archive.org/web/20190224205626/http://www.mkyong.com/tag/jdbc/) [jsf2](http://web.archive.org/web/20190224205626/http://www.mkyong.com/tag/jsf2/)![](img/b24a66e466723f164fed85e4078b47e8.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224205626/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7855">







