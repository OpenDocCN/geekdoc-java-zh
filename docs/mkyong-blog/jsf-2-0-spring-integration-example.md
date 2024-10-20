# JSF 2 + Spring 3 集成示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jsf2/jsf-2-0-spring-integration-example/>

在本教程中，我们将向您展示如何使用以下工具将 JSF 2.0 与 Spring 3 集成:

1.  JSF XML faces-config.xml
2.  弹簧注释
3.  JSR-330 标准注射液

使用的工具和技术:

1.  JSF 2.1.13
2.  释放弹簧
3.  maven3
4.  Eclipse 4.2
5.  Tomcat 6 或 7

## 1.目录结构

用于演示的标准 Maven 项目。



![jsf2-spring-example-folder](img/1e4f7e7852ad154ebfc02351d6ff3fea.png "jsf2-spring-example-folder")

## 2.项目相关性

声明 JSF 2，春天 3，JSR 330 注入，和 Tomcat 的依赖。

pom.xml

```java
 <project  
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
  	http://maven.apache.org/maven-v4_0_0.xsd">

	<modelVersion>4.0.0</modelVersion>
	<groupId>com.mkyong.common</groupId>
	<artifactId>JavaServerFaces</artifactId>
	<packaging>war</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>JavaServerFaces Maven Webapp</name>
	<url>http://maven.apache.org</url>

	<dependencies>

		<!-- Spring framework -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>3.1.2.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>3.1.2.RELEASE</version>
		</dependency>

		<!-- JSR-330 -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- JSF -->
		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-api</artifactId>
			<version>2.1.13</version>
		</dependency>
		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-impl</artifactId>
			<version>2.1.13</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
		</dependency>

		<!-- EL -->
		<dependency>
			<groupId>org.glassfish.web</groupId>
			<artifactId>el-impl</artifactId>
			<version>2.2</version>
		</dependency>

		<!-- Tomcat 6 need this -->
		<dependency>
			<groupId>com.sun.el</groupId>
			<artifactId>el-ri</artifactId>
			<version>1.0</version>
		</dependency>

	</dependencies>

	<build>
	<finalName>JavaServerFaces</finalName>

	<plugins>
	   <plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-compiler-plugin</artifactId>
		<version>2.3.1</version>
		<configuration>
			<source>1.6</source>
			<target>1.6</target>
		</configuration>
	   </plugin>
	</plugins>
	</build>
</project> 
```

## 3.JSF 2 + Spring 集成

Spring Ioc 环境下的 Spring bean 和 JSF Ioc 环境下的 JSF 托管 bean，如何让两者协同工作？解决方法是在`faces-config.xml`中定义弹簧的`SpringBeanFacesELResolver`。检查这个[官方弹簧指南](http://web.archive.org/web/20201112012809/http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html/web-integration.html#jsf-springbeanfaceselresolver)。

faces-config.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<faces-config 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-facesconfig_2_1.xsd"
	version="2.1">

	<application>
		<el-resolver>
    		    org.springframework.web.jsf.el.SpringBeanFacesELResolver
		</el-resolver>
  	</application>

</faces-config> 
```

参见下面 3 个例子在 JSF 管理的 bean 中注入 Spring 的 bean。

## 3.1.XML 模式示例

许多开发人员仍然喜欢使用 XML 来管理 beans。使用`SpringBeanFacesELResolver`，只是使用 EL `${userBo}`将 Spring 的 bean 注入到 JSF 的托管 bean 中。

UserBo.java

```java
 package com.mkyong.user.bo;

public interface UserBo{

	public String getMessage();

} 
```

UserBoImpl.java

```java
 package com.mkyong.user.bo.impl;

import com.mkyong.user.bo.UserBo;

public class UserBoImpl implements UserBo{

	public String getMessage() {

		return "JSF 2 + Spring Integration";

	}

} 
```

UserBean.java – JSF backing bean

```java
 package com.mkyong;

import java.io.Serializable;
import com.mkyong.user.bo.UserBo;

public class UserBean{

        //later inject in faces-config.xml
	UserBo userBo;

	public void setUserBo(UserBo userBo) {
		this.userBo = userBo;
	}

	public String printMsgFromSpring() {

		return userBo.getMessage();

	}

} 
```

applicationContext.xml – Declares userBo bean

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="userBo" class="com.mkyong.user.bo.impl.UserBoImpl"></bean>

</beans> 
```

faces-config.xml – Declares managed bean and inject userBo

```java
 <?xml version="1.0" encoding="UTF-8"?>
<faces-config

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
    http://java.sun.com/xml/ns/javaee/web-facesconfig_2_0.xsd"
    version="2.0">

	<managed-bean>
		<managed-bean-name>user</managed-bean-name>
		<managed-bean-class>com.mkyong.UserBean</managed-bean-class>
		<managed-bean-scope>session</managed-bean-scope>
		<managed-property>
			<property-name>userBo</property-name>
			<value>#{userBo}</value>
		</managed-property>
	</managed-bean>

</faces-config> 
```

## 3.2.弹簧注释–自动扫描

这个例子使用了 Spring 注释。像普通的 bean 一样注入`@ManagedBean`、`@Autowired`和`@Component`，它就像预期的那样工作。

UserBoImpl.java

```java
 package com.mkyong.user.bo.impl;

import org.springframework.stereotype.Service;
import com.mkyong.user.bo.UserBo;

@Service
public class UserBoImpl implements UserBo{

	public String getMessage() {

		return "JSF 2 + Spring Integration";

	}

} 
```

UserBean.java

```java
 package com.mkyong;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import com.mkyong.user.bo.UserBo;

@Component
@ManagedBean
@SessionScoped
public class UserBean{

	@Autowired
	UserBo userBo;

	public void setUserBo(UserBo userBo) {
		this.userBo = userBo;
	}

	public String printMsgFromSpring() {
		return userBo.getMessage();
	}

} 
```

applicationContext.xml – Enable the component auto scan

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.1.xsd">

	<context:component-scan base-package="com.mkyong" />

</beans> 
```

**混合使用 JSF 和春天注释**工作正常，但是看起来很奇怪而且重复——`@Component`和`@ManagedBean`在一起。实际上，你可以只用一个`@Component`，看下面的新版本，它是纯弹簧的，而且很有效！

UserBean.java

```java
 package com.mkyong;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

import com.mkyong.user.bo.UserBo;

@Component
@Scope("session")
public class UserBean{

	@Autowired
	UserBo userBo;

	public void setUserBo(UserBo userBo) {
		this.userBo = userBo;
	}

	public String printMsgFromSpring() {
		return userBo.getMessage();
	}

} 
```

## 3.3.JSR-330 注解

从 Spring 3.0 开始， [Spring 为 JSR-330 注入标准](http://web.archive.org/web/20201112012809/http://www.mkyong.com/spring3/spring-3-and-jsr-330-inject-and-named-example/)提供支持。现在，你可以用`@Inject`代替`@Autowired`，用`@Named`代替`@Component`。这是推荐的解决方案，遵循 JSR-330 标准使应用程序更容易移植到其他环境，它在 Spring 框架中工作良好。

UserBoImpl.java

```java
 package com.mkyong.user.bo.impl;

import javax.inject.Named;
import com.mkyong.user.bo.UserBo;

@Named
public class UserBoImpl implements UserBo{

	public String getMessage() {

		return "JSF 2 + Spring Integration";

	}

} 
```

UserBean.java

```java
 package com.mkyong;

import javax.inject.Inject;
import javax.inject.Named;
import org.springframework.context.annotation.Scope;
import com.mkyong.user.bo.UserBo;

@Named
@Scope("session") //need this, JSR-330 in Spring context is singleton by default
public class UserBean {

	@Inject
	UserBo userBo;

	public void setUserBo(UserBo userBo) {
		this.userBo = userBo;
	}

	public String printMsgFromSpring() {
		return userBo.getMessage();
	}

} 
```

applicationContext.xml – Need component auto scan also

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.1.xsd">

	<context:component-scan base-package="com.mkyong" />

</beans> 
```

## 4.演示

**3.1** 、 **3.2** 和 **3.3** 中的例子都在做同样的事情——将`userBo`注入 JSF 豆，只是实现方式不同。现在，创建一个简单的 JSF 页面来显示结果。

default.xhtml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      >

    <h:body>

    	<h1>JSF 2.0 + Spring Example</h1>

 	#{userBean.printMsgFromSpring()}

    </h:body>

</html> 
```

web.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 

	xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" 
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" 
	id="WebApp_ID" version="2.5">

  <display-name>JavaServerFaces</display-name>

  <!-- Add Support for Spring -->
  <listener>
	<listener-class>
		org.springframework.web.context.ContextLoaderListener
	</listener-class>
  </listener>
  <listener>
	<listener-class>
		org.springframework.web.context.request.RequestContextListener
	</listener-class>
  </listener>

  <!-- Change to "Production" when you are ready to deploy -->
  <context-param>
    <param-name>javax.faces.PROJECT_STAGE</param-name>
    <param-value>Development</param-value>
  </context-param>

  <!-- Welcome page -->
  <welcome-file-list>
    <welcome-file>default.jsf</welcome-file>
  </welcome-file-list>

  <!-- JSF Mapping -->
  <servlet>
    <servlet-name>facesServlet</servlet-name>
    <servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>facesServlet</servlet-name>
    <url-pattern>*.jsf</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
    <servlet-name>facesServlet</servlet-name>
    <url-pattern>*.xhtml</url-pattern>
  </servlet-mapping>

</web-app> 
```

完成，查看输出:*http://localhost:8080/Java server faces/default . JSF*



![jsf2 and spring integration](img/43d088e34fa5def5d560f247bbb2d73e.png "jsf2-spring")

## 下载源代码

Download It – [JSF2-Spring-Example.zip](http://web.archive.org/web/20201112012809/http://www.mkyong.com/wp-content/uploads/2010/12/JSF2-Spring-Example.zip) (31KB)

## 参考

1.  [弹簧参考–弹簧面选择分解器](http://web.archive.org/web/20201112012809/http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html/web-integration.html#jsf-springbeanfaceselresolver)
2.  [Spring 如何在会话监听器中进行依赖注入](http://web.archive.org/web/20201112012809/http://www.mkyong.com/spring/spring-how-to-do-dependency-injection-in-your-session-listener/)
3.  [弹簧 3 和 JSR-330 @注入和@命名示例](http://web.archive.org/web/20201112012809/http://www.mkyong.com/spring3/spring-3-and-jsr-330-inject-and-named-example/)

Tags : [integration](http://web.archive.org/web/20201112012809/https://mkyong.com/tag/integration/) [jsf2](http://web.archive.org/web/20201112012809/https://mkyong.com/tag/jsf2/) [spring](http://web.archive.org/web/20201112012809/https://mkyong.com/tag/spring/) [spring3](http://web.archive.org/web/20201112012809/https://mkyong.com/tag/spring3/)<input type="hidden" id="mkyong-current-postId" value="7839">

### 相关文章

*   [Spring 3 + Quartz 1.8.6 调度器示例](/web/20201112012809/https://www.mkyong.com/spring/spring-quartz-scheduler-example/)
*   [JSF 2.0 + Spring + Hibernate 集成实例](/web/20201112012809/https://www.mkyong.com/jsf2/jsf-2-0-spring-hibernate-integration-example/)
*   [Wicket + Spring 集成示例](/web/20201112012809/https://www.mkyong.com/wicket/wicket-spring-integration-example/)
*   [春天+ JDBC 的例子](/web/20201112012809/https://www.mkyong.com/spring/maven-spring-jdbc-example/)
*   [Spring+JDBC template+JdbcDaoSupport 示例](/web/20201112012809/https://www.mkyong.com/spring/spring-jdbctemplate-jdbcdaosupport-examples/)

*   [Spring + JDK 定时器调度器示例](/web/20201112012809/https://www.mkyong.com/spring/spring-jdk-timer-scheduler-example/)
*   [Spring @PropertySource 示例](/web/20201112012809/https://www.mkyong.com/spring/spring-propertysources-example/)
*   [春季教程](/web/20201112012809/https://www.mkyong.com/tutorials/spring-tutorials/)
*   [Struts + Spring 集成示例](/web/20201112012809/https://www.mkyong.com/struts/struts-spring-integration-example/)
*   [Struts + Spring + Hibernate 集成示例](/web/20201112012809/https://www.mkyong.com/struts/struts-spring-hibernate-integration-example/)