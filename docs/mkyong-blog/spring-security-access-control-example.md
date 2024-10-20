# Spring 安全访问控制示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-security/spring-security-access-control-example/>

在 Spring Security 中，访问控制或授权很容易实现。请参见以下代码片段:

```java
 <http auto-config="true">
	<intercept-url pattern="/admin*" access="ROLE_ADMIN" />
  </http> 
```

也就是说，只有权限为“ **ROLE_ADMIN** 的用户才能访问 URI **/admin*** 。如果非授权用户试图访问它，将显示“ **http 403 访问被拒绝页面**”。

**Spring EL + Access Control**
See equivalent version in Spring EL. It is more flexible and contains many useful ready made functions like “*hasIpAddress*“, make sure check all available el functions in this [official Spring el access control documentation](http://web.archive.org/web/20190215001531/http://static.springsource.org/spring-security/site/docs/3.0.x/reference/el-access.html).

```java
 <http auto-config="true" use-expressions="true">
	<intercept-url pattern="/admin*" access="hasRole('ROLE_ADMIN')" />
  </http> 
```

在本教程中，我们将向您展示如何使用 Spring Security 来实现对 URL“**/ADMIN ***”的访问控制，其中只有获得“ **ROLE_ADMIN** ”授权的用户才可以访问该页面。

## 1.项目相关性

访问控制包含在核心 Spring 安全 jar 中。请参考这个 [Spring Security hello world 示例](http://web.archive.org/web/20190215001531/http://www.mkyong.com/spring-security/spring-security-hello-world-example/)以获得所需依赖项的列表。

 ## 2.Spring MVC

Spring MVC 控制器并返回一个“hello”视图，这应该是不言自明的。

*文件:WelcomeController.java*

```java
 package com.mkyong.common.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class WelcomeController {

	@RequestMapping(value = "/admin", method = RequestMethod.GET)
	public String welcomeAdmin(ModelMap model) {

		model.addAttribute("message", "Spring Security - ROLE_ADMIN");
		return "hello";

	}

} 
```

*文件:hello.jsp*

```java
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
<body>
	<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Message : ${message}</h2>	

	<a href="<c:url value="j_spring_security_logout" />" > Logout</a>
</body>
</html> 
```

## 3.春天安全

完整的 Spring 安全配置，只允许用户" **eclipse** "访问" **/admin** "页面。

```java
 <beans:beans 
	xmlns:beans="http://www.springframework.org/schema/beans" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/security
	http://www.springframework.org/schema/security/spring-security-3.0.3.xsd">

	<http auto-config="true">
		<intercept-url pattern="/admin*" access="ROLE_ADMIN" />
		<logout logout-success-url="/admin" />
	</http>

	<authentication-manager>
	  <authentication-provider>
	   <user-service>
		<user name="mkyong" password="password" authorities="ROLE_USER" />
		<user name="eclipse" password="password" authorities="ROLE_ADMIN" />
	   </user-service>
	  </authentication-provider>
	</authentication-manager>

</beans:beans> 
```

## 4.演示

网址:*http://localhost:8080/spring MVC/admin*

1.将显示默认登录表单。

![demo page - access control](img/0f7b6d6dd62d422f21d68ba3dd74957e.png "spring-security-access-control-login")

2.如果用户“ **mkyong** ”登录，会显示“ **http 403 被拒绝访问页面**，因为“ **mkyong** ”是“ **ROLE_USER** ”。

![demo page - access denied](img/bdf8d2ffea24b67d36bd0e25a807eebe.png "spring-security-access-control-denied")

3.如果用户“ **eclipse** ”登录，会显示“【hello.jsp】T2，因为“ **eclipse** ”是“ **ROLE_ADMIN** ”。

![demo page - success](img/79e3e0741e5256f6c93d7cc36114dc39.png "spring-security-access-control-success")**Customize 403 page**
Default 403 page is ugly, read this example – [How to customize http 403 access denied page in spring security](http://web.archive.org/web/20190215001531/http://www.mkyong.com/spring-security/customize-http-403-access-denied-page-in-spring-security/).

## 下载源代码

Download it – [Spring-Security-Access-Control-Example.zip](http://web.archive.org/web/20190215001531/http://www.mkyong.com/wp-content/uploads/2011/08/Spring-Security-Access-Control-Example.zip) (10 KB)

## 参考

1.  [Spring 安全授权文档](http://web.archive.org/web/20190215001531/http://static.springsource.org/spring-security/site/docs/3.0.x/reference/authorization.html)
2.  [Spring 安全+ el 访问控制文档](http://web.archive.org/web/20190215001531/http://static.springsource.org/spring-security/site/docs/3.0.x/reference/el-access.html)

[access control](http://web.archive.org/web/20190215001531/http://www.mkyong.com/tag/access-control/) [spring security](http://web.archive.org/web/20190215001531/http://www.mkyong.com/tag/spring-security/)![](img/ced56df2a4f8ea0f775b51c99165f3c8.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190215001531/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="10075">







