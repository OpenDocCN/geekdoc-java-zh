# 使用数据库的 Spring 安全表单登录

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-security/spring-security-form-login-using-database/>

在本教程中，我们将向您展示如何在 Spring Security 中执行数据库认证(使用 XML 和注释)。

使用的技术:

1.  弹簧 3.2.8 释放
2.  Spring Security 3.2.3 .发布
3.  春季 JDBC 3.2.3 .发布
4.  Eclipse 4.2
5.  JDK 1.6
6.  maven3
7.  Tomcat 6 或 7 (Servlet 3.x)
8.  MySQL 服务器 5.6

以前的[登录表单内存认证](http://web.archive.org/web/20220323160657/http://www.mkyong.com/spring-security/spring-security-form-login-example/)将被重用，增强以支持以下特性:

1.  数据库认证，使用 Spring-JDBC 和 MySQL。
2.  Spring Security，JSP TagLib，`sec:authorize access="hasRole('ROLE_USER')`
3.  自定义 403 拒绝访问页面。

## 1.项目演示

[//web.archive.org/web/20220323160657if_/https://www.youtube.com/embed/2ms57c2EdUg](//web.archive.org/web/20220323160657if_/https://www.youtube.com/embed/2ms57c2EdUg)

## 2.项目目录

查看最终项目结构(基于 XML):

![spring-security-database-xml-directory](img/27640af7c07b9126bbd7396655b488fb.png)

查看最终项目结构(基于注释):

![spring-security-database-annotation-directory](img/caff60c806f12e9d5583e622a94ae543.png)

## 3.项目相关性

获取对 Spring、Spring Security、JDBC、Taglib 和 MySQL 的依赖

pom.xml

```java
 <properties>
		<jdk.version>1.6</jdk.version>
		<spring.version>3.2.8.RELEASE</spring.version>
		<spring.security.version>3.2.3.RELEASE</spring.security.version>
		<jstl.version>1.2</jstl.version>
		<mysql.connector.version>5.1.30</mysql.connector.version>
	</properties>

	<dependencies>

		<!-- Spring 3 dependencies -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- Spring Security -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-web</artifactId>
			<version>${spring.security.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-config</artifactId>
			<version>${spring.security.version}</version>
		</dependency>

		<!-- Spring Security JSP Taglib -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-taglibs</artifactId>
			<version>${spring.security.version}</version>
		</dependency>

		<!-- jstl for jsp page -->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
		</dependency>

                <!-- connect to mysql -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${mysql.connector.version}</version>
		</dependency>

	</dependencies>

</project> 
```

## 4.数据库ˌ资料库

要执行数据库身份验证，您必须创建表来存储用户和角色的详细信息。请参考这个 [Spring 安全用户模式参考](http://web.archive.org/web/20220323160657/https://docs.spring.io/spring-security/site/docs/3.2.3.RELEASE/reference/htmlsingle/#user-schema)。下面是创建`users`和`user_roles`表的 MySQL 脚本。

4.1 创建一个“用户”表。

users.sql

```java
 CREATE  TABLE users (
  username VARCHAR(45) NOT NULL ,
  password VARCHAR(45) NOT NULL ,
  enabled TINYINT NOT NULL DEFAULT 1 ,
  PRIMARY KEY (username)); 
```

4.2 创建一个“用户角色”表。

user_roles.sql

```java
 CREATE TABLE user_roles (
  user_role_id int(11) NOT NULL AUTO_INCREMENT,
  username varchar(45) NOT NULL,
  role varchar(45) NOT NULL,
  PRIMARY KEY (user_role_id),
  UNIQUE KEY uni_username_role (role,username),
  KEY fk_username_idx (username),
  CONSTRAINT fk_username FOREIGN KEY (username) REFERENCES users (username)); 
```

4.3 插入一些测试记录。

```java
 INSERT INTO users(username,password,enabled)
VALUES ('mkyong','123456', true);
INSERT INTO users(username,password,enabled)
VALUES ('alex','123456', true);

INSERT INTO user_roles (username, role)
VALUES ('mkyong', 'ROLE_USER');
INSERT INTO user_roles (username, role)
VALUES ('mkyong', 'ROLE_ADMIN');
INSERT INTO user_roles (username, role)
VALUES ('alex', 'ROLE_USER'); 
```

**Note**

1.  用户名“mkyong”，角色 _ 用户和角色 _ 管理员。
2.  用户名“alexa”，带角色 _ 用户。

## 5.Spring 安全配置

XML 和注释中的 Spring 安全性。

5.1 创建一个数据源来连接 MySQL。

spring-database.xml

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">

		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/test" />
		<property name="username" value="root" />
		<property name="password" value="password" />
	</bean>

</beans> 
```

相当于弹簧注释:

SecurityConfig.java

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import org.springframework.web.servlet.view.JstlView;

@EnableWebMvc
@Configuration
@ComponentScan({ "com.mkyong.web.*" })
@Import({ SecurityConfig.class })
public class AppConfig {

	@Bean(name = "dataSource")
	public DriverManagerDataSource dataSource() {
	    DriverManagerDataSource driverManagerDataSource = new DriverManagerDataSource();
	    driverManagerDataSource.setDriverClassName("com.mysql.jdbc.Driver");
	    driverManagerDataSource.setUrl("jdbc:mysql://localhost:3306/test");
	    driverManagerDataSource.setUsername("root");
	    driverManagerDataSource.setPassword("password");
	    return driverManagerDataSource;
	}

	@Bean
	public InternalResourceViewResolver viewResolver() {
	    InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
	    viewResolver.setViewClass(JstlView.class);
	    viewResolver.setPrefix("/WEB-INF/pages/");
	    viewResolver.setSuffix(".jsp");
	    return viewResolver;
	}

} 
```

5.2 使用`jdbc-user-service`定义一个执行数据库认证的查询。

spring-security.xml

```java
 <beans:beans 
	xmlns:beans="http://www.springframework.org/schema/beans" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/security
	http://www.springframework.org/schema/security/spring-security-3.2.xsd">

    <!-- enable use-expressions -->
	<http auto-config="true" use-expressions="true">

		<intercept-url pattern="/admin**" access="hasRole('ROLE_ADMIN')" />

		<!-- access denied page -->
		<access-denied-handler error-page="/403" />

		<form-login 
		    login-page="/login" 
		    default-target-url="/welcome" 
			authentication-failure-url="/login?error" 
			username-parameter="username"
			password-parameter="password" />
		<logout logout-success-url="/login?logout"  />
		<!-- enable csrf protection -->
		<csrf/>
	</http>

	<!-- Select users and user_roles from database -->
	<authentication-manager>
	  <authentication-provider>
		<jdbc-user-service data-source-ref="dataSource"
		  users-by-username-query=
		    "select username,password, enabled from users where username=?"
		  authorities-by-username-query=
		    "select username, role from user_roles where username =?  " />
	  </authentication-provider>
	</authentication-manager>

</beans:beans> 
```

相当于 Spring 安全注释:

SecurityConfig.java

```java
 package com.mkyong.config;

import javax.sql.DataSource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Autowired
	DataSource dataSource;

	@Autowired
	public void configAuthentication(AuthenticationManagerBuilder auth) throws Exception {

	  auth.jdbcAuthentication().dataSource(dataSource)
		.usersByUsernameQuery(
			"select username,password, enabled from users where username=?")
		.authoritiesByUsernameQuery(
			"select username, role from user_roles where username=?");
	}	

	@Override
	protected void configure(HttpSecurity http) throws Exception {

	  http.authorizeRequests()
		.antMatchers("/admin/**").access("hasRole('ROLE_ADMIN')")
		.and()
		  .formLogin().loginPage("/login").failureUrl("/login?error")
		  .usernameParameter("username").passwordParameter("password")
		.and()
		  .logout().logoutSuccessUrl("/login?logout")
		.and()
		  .exceptionHandling().accessDeniedPage("/403")
		.and()
		  .csrf();
	}
} 
```

## 6.JSP 页面

定制登录页面的 JSP 页面。

6.1 默认页面，展示使用 Spring Security JSP taglib `sec:authorize`向拥有“ **ROLE_USER** 权限的用户显示内容。

hello.jsp

```java
 <%@taglib prefix="sec"
	uri="http://www.springframework.org/security/tags"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
<body>
	<h1>Title : ${title}</h1>
	<h1>Message : ${message}</h1>

	<sec:authorize access="hasRole('ROLE_USER')">
		<!-- For login user -->
		<c:url value="/j_spring_security_logout" var="logoutUrl" />
		<form action="${logoutUrl}" method="post" id="logoutForm">
			<input type="hidden" name="${_csrf.parameterName}"
				value="${_csrf.token}" />
		</form>
		<script>
			function formSubmit() {
				document.getElementById("logoutForm").submit();
			}
		</script>

		<c:if test="${pageContext.request.userPrincipal.name != null}">
			<h2>
				User : ${pageContext.request.userPrincipal.name} | <a
					href="javascript:formSubmit()"> Logout</a>
			</h2>
		</c:if>

	</sec:authorize>
</body>
</html> 
```

6.2 显示自定义登录表单的页面。

login.jsp

```java
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@page session="true"%>
<html>
<head>
<title>Login Page</title>
<style>
.error {
	padding: 15px;
	margin-bottom: 20px;
	border: 1px solid transparent;
	border-radius: 4px;
	color: #a94442;
	background-color: #f2dede;
	border-color: #ebccd1;
}

.msg {
	padding: 15px;
	margin-bottom: 20px;
	border: 1px solid transparent;
	border-radius: 4px;
	color: #31708f;
	background-color: #d9edf7;
	border-color: #bce8f1;
}

#login-box {
	width: 300px;
	padding: 20px;
	margin: 100px auto;
	background: #fff;
	-webkit-border-radius: 2px;
	-moz-border-radius: 2px;
	border: 1px solid #000;
}
</style>
</head>
<body onload='document.loginForm.username.focus();'>

	<h1>Spring Security Login Form (Database Authentication)</h1>

	<div id="login-box">

		<h2>Login with Username and Password</h2>

		<c:if test="${not empty error}">
			<div class="error">${error}</div>
		</c:if>
		<c:if test="${not empty msg}">
			<div class="msg">${msg}</div>
		</c:if>

		<form name='loginForm'
		  action="<c:url value='/j_spring_security_check' />" method='POST'>

		<table>
			<tr>
				<td>User:</td>
				<td><input type='text' name='username'></td>
			</tr>
			<tr>
				<td>Password:</td>
				<td><input type='password' name='password' /></td>
			</tr>
			<tr>
				<td colspan='2'><input name="submit" type="submit"
				  value="submit" /></td>
			</tr>
		  </table>

		  <input type="hidden" name="${_csrf.parameterName}"
			value="${_csrf.token}" />

		</form>
	</div>

</body>
</html> 
```

6.3 该页面受密码保护，只有经过认证的用户“ **ROLE_ADMIN** ”可以访问。

admin.jsp

```java
 <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@page session="true"%>
<html>
<body>
	<h1>Title : ${title}</h1>
	<h1>Message : ${message}</h1>

	<c:url value="/j_spring_security_logout" var="logoutUrl" />
	<form action="${logoutUrl}" method="post" id="logoutForm">
		<input type="hidden" name="${_csrf.parameterName}"
			value="${_csrf.token}" />
	</form>
	<script>
		function formSubmit() {
			document.getElementById("logoutForm").submit();
		}
	</script>

	<c:if test="${pageContext.request.userPrincipal.name != null}">
		<h2>
			Welcome : ${pageContext.request.userPrincipal.name} | <a
				href="javascript:formSubmit()"> Logout</a>
		</h2>
	</c:if>

</body>
</html> 
```

6.4 自定义 403 拒绝访问页面。

403.jsp

```java
 <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
<body>
	<h1>HTTP Status 403 - Access is denied</h1>

	<c:choose>
		<c:when test="${empty username}">
		  <h2>You do not have permission to access this page!</h2>
		</c:when>
		<c:otherwise>
		  <h2>Username : ${username} <br/>
                    You do not have permission to access this page!</h2>
		</c:otherwise>
	</c:choose>

</body>
</html> 
```

## 7.Spring MVC 控制器

简单的控制器。

MainController.java

```java
 package com.mkyong.web.controller;

import org.springframework.security.authentication.AnonymousAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class MainController {

	@RequestMapping(value = { "/", "/welcome**" }, method = RequestMethod.GET)
	public ModelAndView defaultPage() {

	  ModelAndView model = new ModelAndView();
	  model.addObject("title", "Spring Security Login Form - Database Authentication");
	  model.addObject("message", "This is default page!");
	  model.setViewName("hello");
	  return model;

	}

	@RequestMapping(value = "/admin**", method = RequestMethod.GET)
	public ModelAndView adminPage() {

	  ModelAndView model = new ModelAndView();
	  model.addObject("title", "Spring Security Login Form - Database Authentication");
	  model.addObject("message", "This page is for ROLE_ADMIN only!");
	  model.setViewName("admin");
	  return model;

	}

	@RequestMapping(value = "/login", method = RequestMethod.GET)
	public ModelAndView login(@RequestParam(value = "error", required = false) String error,
		@RequestParam(value = "logout", required = false) String logout) {

	  ModelAndView model = new ModelAndView();
	  if (error != null) {
		model.addObject("error", "Invalid username and password!");
	  }

	  if (logout != null) {
		model.addObject("msg", "You've been logged out successfully.");
	  }
	  model.setViewName("login");

	  return model;

	}

	//for 403 access denied page
	@RequestMapping(value = "/403", method = RequestMethod.GET)
	public ModelAndView accesssDenied() {

	  ModelAndView model = new ModelAndView();

	  //check if user is login
	  Authentication auth = SecurityContextHolder.getContext().getAuthentication();
	  if (!(auth instanceof AnonymousAuthenticationToken)) {
		UserDetails userDetail = (UserDetails) auth.getPrincipal();	
		model.addObject("username", userDetail.getUsername());
	  }

	  model.setViewName("403");
	  return model;

	}

} 
```

## 8.演示

8.1.默认页面
XML—*http://localhost:8080/spring-security-loginform-database/*
Annotation—*http://localhost:8080/spring-security-loginform-database-Annotation/*

![spring-security-database-default](img/6d30474adbadc5007f24e785d5e85a9d.png)

8.2 尝试访问`/admin`页面，只允许“mkyong”**ROLE _ ADMIN**访问。

![spring-security-database-admin](img/2ac65692d5bca7979ed7d6819cb97153.png)

8.3.如果“alex”试图访问`/admin`，则显示 403 访问被拒绝页面。

![spring-security-database-403](img/af91f5c3b5f769caac4d0856c6f508c3.png)

8.3 "alex "在默认页面中显示`sec:authorize`的用法

![spring-security-database-sec-tag](img/411f29299f2595bbb93c97ad0fae8032.png)

8.4.如果“mkyong”试图访问`/admin`，将显示管理页面。

![spring-security-database-admin-login](img/b359d183344b8b904f7eaff82d4dfb42.png)

## 下载源代码

Download XML version – [spring-security-login-form-database-xml.zip](http://web.archive.org/web/20220323160657/http://www.mkyong.com/wp-content/uploads/2011/08/spring-security-login-form-database-xml.zip) (16 KB)Download Annotation version – [spring-security-login-form-database-annotation.zip](http://web.archive.org/web/20220323160657/http://www.mkyong.com/wp-content/uploads/2011/08/spring-security-login-form-database-annotation.zip) (22 KB)

## 参考

1.  [Spring 安全参考–JDBC-ser-service](http://web.archive.org/web/20220323160657/https://docs.spring.io/spring-security/site/docs/3.2.3.RELEASE/reference/htmlsingle/#nsa-jdbc-user-service) 
2.  [Spring 安全参考–juser-schema](http://web.archive.org/web/20220323160657/https://docs.spring.io/spring-security/site/docs/3.2.3.RELEASE/reference/htmlsingle/#user-schema)
3.  [Spring 安全参考–标签库](http://web.archive.org/web/20220323160657/https://docs.spring.io/spring-security/site/docs/3.2.3.RELEASE/reference/htmlsingle/#taglibs)
4.  [春安你好世界注释示例](http://web.archive.org/web/20220323160657/http://www.mkyong.com/spring-security/spring-security-hello-world-annotation-example/)
5.  [创建自定义登录表单](http://web.archive.org/web/20220323160657/https://docs.spring.io/spring-security/site/docs/3.2.0.RELEASE/guides/form.html)
6.  [Spring Security–如何定制 403 拒绝访问页面](http://web.archive.org/web/20220323160657/http://www.mkyong.com/spring-security/customize-http-403-access-denied-page-in-spring-security/)

<input type="hidden" id="mkyong-current-postId" value="10055">