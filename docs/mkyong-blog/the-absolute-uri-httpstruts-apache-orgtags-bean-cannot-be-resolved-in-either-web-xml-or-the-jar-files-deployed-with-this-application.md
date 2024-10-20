# 绝对 uri:http://struts.apache.org/tags-bean 无法在 web.xml 或与此应用程序一起部署的 jar 文件中解析。

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/the-absolute-uri-httpstruts-apache-orgtags-bean-cannot-be-resolved-in-either-web-xml-or-the-jar-files-deployed-with-this-application/>

## 问题

访问 Struts 标记库 JSP 文件时的 Struts 常见错误消息。

```java
<%@taglib uri="http://struts.apache.org/tags-bean" prefix="bean"%>

```

```java
 org.apache.jasper.JasperException: The absolute uri: 
http://struts.apache.org/tags-bean 

org.apache.jasper.JasperException: The absolute uri: 
http://struts.apache.org/tags-html 

org.apache.jasper.JasperException: The absolute uri: 
http://struts.apache.org/tags-logic 

org.apache.jasper.JasperException: The absolute uri: 
http://struts.apache.org/tags-tiles 

cannot be resolved in either web.xml or 
the jar files deployed with this application 
```

 ## 解决办法

这是因为您没有配置 Struts 标记库属性，在您的项目依赖项中找不到 tld 文件。

 ## 1.支柱标记库手动配置

如果你是手动配置 Struts 标签库，用在 **Struts 版本< = 1.1 和 Servlet < 2.3 容器**中。请确保将下面的“ **tld** 文件复制到 **WEB-INF** 文件夹中，您可以在您的 Struts 库文件夹中找到这些文件。

*   struts-bean.tld
*   struts-html.tld
*   struts-logic.tld
*   struts-tiles.tld

并在 web.xml
**web.xml** 中适当定义

```java
 ...
<taglib>
     <taglib-uri>
	  http://struts.apache.org/tags-bean
     </taglib-uri>
     <taglib-location>
	  /WEB-INF/struts-bean.tld
     </taglib-location>
</taglib>
... 
```

## 2.支柱标签库自动配置

如果你是自动配置 Struts 标签库，用在**Servlet 2.3/2.4 版和 Struts 1.2 或 1.3** 框架中。确保 **struts-taglib.jar** 在你的 **/WEB-INF/lib** 目录中。

你可以在这里查看 [Struts 标签库的配置细节。](http://web.archive.org/web/20190220115711/http://www.mkyong.com/struts/configure-the-struts-tag-libraries/)

## 3.Eclipse IDE 调试会话

如果这是在 Eclipse IDE 调试会话期间发生的，只需确保您的项目依赖项被部署到正确的文件夹。解决方案请查看[这篇文章。](http://web.archive.org/web/20190220115711/http://www.mkyong.com/maven/maven-dependency-libraries-not-deploy-in-eclipse-ide/)

## 结论

问题的原因可能因项目而异，但解决方案总是相同的

*   检查 WEB-INF 中的 tld 文件是否可用(旧的 Struts 样式)。
*   请检查标记库 uri 没有打字错误。
*   检查 **struts-taglib.jar** 在 **/WEB-INF/lib** 或项目依赖(新的 struts 样式)中是否可用。

[struts](http://web.archive.org/web/20190220115711/http://www.mkyong.com/tag/struts/)







