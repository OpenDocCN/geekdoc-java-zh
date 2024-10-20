# Spring 安全注销示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-security/spring-security-logout-example/>

在 Spring Security 中，要注销，只需添加一个链接到 URL“**j _ Spring _ Security _ logout**”，例如:

```java
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
<body>
	<h2>messages, whatever</h2>	
	<a href="<c:url value="j_spring_security_logout" />" > Logout</a>
</body>
</html> 
```

在 Spring security 中，声明“`logout`”标签，并配置“`logout-success-url`”属性:

```java
 <beans:beans 
	xmlns:beans="http://www.springframework.org/schema/beans" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/security
	http://www.springframework.org/schema/security/spring-security-3.0.3.xsd">

	<http auto-config="true">
		<intercept-url pattern="/welcome*" access="ROLE_USER" />
		<logout logout-success-url="/welcome" />
	</http>

	<authentication-manager>
	  <authentication-provider>
	    <user-service>
		<user name="mkyong" password="password" authorities="ROLE_USER" />
	    </user-service>
	  </authentication-provider>
	</authentication-manager>

</beans:beans> 
```

 ## 下载源代码

Download it – [Spring-Security-LogOut-Example.zip](http://web.archive.org/web/20190301115812/http://www.mkyong.com/wp-content/uploads/2011/08/Spring-Security-LogOut-Example.zip) (8 KB)[logout](http://web.archive.org/web/20190301115812/http://www.mkyong.com/tag/logout/) [spring security](http://web.archive.org/web/20190301115812/http://www.mkyong.com/tag/spring-security/)![](img/1c4d88c4b88777c0de3b7fdfa0bd2f17.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190301115812/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="10092">







