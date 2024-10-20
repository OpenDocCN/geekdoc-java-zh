# JSF 2.0 + Tomcat:看来容器的 JSP 版本比 2.1 还要旧…

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-0-tomcat-it-appears-the-jsp-version-of-the-container-is-older-than-2-1/>

## 问题

将 JSF 2.0 web 应用程序部署到 Tomcat 6.0.26 时，遇到“ **JSP 版本的容器比 2.1** 旧”异常，无法启动 Tomcat 服务器。但是 **JSP api v2.1** 包含在项目类路径中，为什么 Tomcat 还在说 JSP 版本比 2.1 老？

```java
 <dependency>
	 <groupId>javax.servlet.jsp</groupId>
	 <artifactId>jsp-api</artifactId>
	 <version>2.1</version>
    </dependency> 
```

这是错误堆栈…

```java
 SEVERE: Critical error during deployment: 
   ...
Caused by: com.sun.faces.config.ConfigurationException: 
It appears the JSP version of the container is older than 2.1 and unable to 
locate the EL RI expression factory, com.sun.el.ExpressionFactoryImpl. 

If not using JSP or the EL RI, make sure the context initialization parameter, 
com.sun.faces.expressionFactory, is properly set. 
```

 ## 解决办法

不太确定它的根本原因，但解决方案是包含 **el-ri.jar** 库

```java
 <dependency>
     <groupId>com.sun.el</groupId>
     <artifactId>el-ri</artifactId>
     <version>1.0</version>
  </dependency> 
```

这个 el-ri.jar 在默认的 [Maven 中央存储库](http://web.archive.org/web/20190228163124/http://repo1.maven.org/maven2/)中可用。

**Note**
The [JSF 2.0 released note](http://web.archive.org/web/20190228163124/https://javaserverfaces.dev.java.net/nonav/rlnotes/2.0.0/releasenotes.html) didn’t mention about this **el-ri.jar** dependency library, that’s weird. ## 更新日期:2010 年 10 月 21 日

这个“el-ri.jar”太旧了，建议使用最新的“el-impl-2.2.jar”，来自[Java.net](http://web.archive.org/web/20190228163124/http://download.java.net/maven/2/org/glassfish/web/el-impl/2.2/el-impl-2.2.pom)

```java
 <dependency>
	  <groupId>org.glassfish.web</groupId>
	  <artifactId>el-impl</artifactId>
	  <version>2.2</version>
     </dependency> 
```

[jsf2](http://web.archive.org/web/20190228163124/http://www.mkyong.com/tag/jsf2/) [jsp](http://web.archive.org/web/20190228163124/http://www.mkyong.com/tag/jsp/) [tomcat](http://web.archive.org/web/20190228163124/http://www.mkyong.com/tag/tomcat/)







