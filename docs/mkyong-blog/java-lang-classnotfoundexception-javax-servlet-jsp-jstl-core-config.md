# Java . lang . classnotfoundexception:javax . servlet . JSP . jstl . core . config

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/java-lang-classnotfoundexception-javax-servlet-jsp-jstl-core-config/>

## 问题

将 JSF 2.0 web 应用程序部署到 Tomcat 6.0.26 时，遇到以下 **jstl** 类未找到错误。

```java
 java.lang.NoClassDefFoundError: javax/servlet/jsp/jstl/core/Config
	...
Caused by: java.lang.ClassNotFoundException: javax.servlet.jsp.jstl.core.Config
	... 18 more 
```

freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_atf", slotId: "mkyong_leaderboard_atf" });

## 解决办法

默认情况下，Tomcat 容器不包含任何 jstl 库。为了修复它，在 Maven `pom.xml`文件中声明 **jstl.jar** 。

```java
 <dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>jstl</artifactId>
	<version>1.2</version>
  </dependency> 
```

**Note**
Please refer to this [JSF 2.0 release note](http://web.archive.org/web/20210329084319/https://javaserverfaces.dev.java.net/nonav/rlnotes/2.0.0/releasenotes.html) to identify the JSF 2.0 required dependency libraries.Tags : [jsf2](http://web.archive.org/web/20210329084319/https://mkyong.com/tag/jsf2/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="6920">