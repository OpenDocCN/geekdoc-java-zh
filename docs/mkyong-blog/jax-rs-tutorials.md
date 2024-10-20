# JAX-遥感教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/jax-rs-tutorials/>

![jax-rs tutorials](img/7474c9efd9598e9dd12102e2178c46e1.png "jaxrs-tutorials")

Java API for RESTful Web Services(**JAX-RS**)，是一套为开发者提供 REST 服务的 API。JAX-RS 是 Java EE6 的一部分，使开发者开发 REST web 应用变得容易。

在这一系列的 JAX-RS 教程中，我们同时使用了 [Jersey](http://web.archive.org/web/20220706180444/https://jersey.java.net/) 和 [RESTEasy](http://web.archive.org/web/20220706180444/http://www.jboss.org/resteasy) ，流行的 JAX-RS 实现。

快乐学习 JAX🙂

## 快速启动

一些快速使用 JAX 遥感器的例子。

*   [Jersey hello world 示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jersey-hello-world-example/)
    Jersey 框架创建一个简单的 REST 风格的 web 应用程序。
*   [RESTEasy hello world 示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/resteasy-hello-world-example/)
    RESTEasy 框架创建一个简单的 REST 风格的 web 应用程序。

## 基本示例

开发 REST 服务的基本注释和函数。

*   [JAX-RS @Path URI 匹配示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jax-rs-path-uri-matching-example/)
    JAX-RS URI 匹配示例。
*   [JAX-RS @PathParam 示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jax-rs-pathparam-example/)
    将@Path 中定义的 URI 参数注入 Java 方法的简单方法。
*   [JAX-RS @QueryParam 示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jax-rs-queryparam-example/)
    示例获取 URI 路径中的查询参数，以及如何定义可选参数。
*   [JAX-RS @MatrixParam 示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jax-rs-matrixparam-example/)
    获取 URI 路径中矩阵参数的示例。
*   [JAX-RS @FormParam 示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jax-rs-formparam-example/)
    获取 HTML post 表单参数值的示例。
*   展示了如何使用@HeaderParam 和@Context 来获取 HTTP 头。
*   [从 JAX 下载文本文件-RS](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/download-text-file-from-jax-rs/)
    示例输出一个文本文件供用户下载。
*   [从 JAX 下载图像文件-RS](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/download-image-file-from-jax-rs/)
    示例输出图像文件供用户下载。
*   [从 JAX 下载 pdf 文件-RS](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/download-pdf-file-from-jax-rs/)
    示例输出 pdf 文件供用户下载。
*   [从 JAX 下载 excel 文件-RS](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/download-excel-file-from-jax-rs/)
    示例输出 excel 文件供用户下载。

## 文件上传示例

如何处理 JAX 遥感中的多部分数据？

*   [文件上传的例子在泽](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/file-upload-example-in-jersey/)
    文件上传在泽很容易。
*   [RESTEasy 中的文件上传示例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/file-upload-example-in-resteasy/)
    RESTEasy 中处理文件上传的两种方式。

## 使用 XML

JAX 遥感系统中的 XML 支持。

*   [XML 示例用 Jersey + JAXB](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/download-xml-with-jersey-jaxb/)
    Jersey + JAXB 将对象映射到 XML 和从 XML 映射。
*   [XML 示例用 RESTEasy+JAXB](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/download-xml-file-from-jax-rs-with-jaxb-resteasy/)
    RESTEasy+JAXB 将对象映射到 XML 和从 XML 映射。

## 使用 JSON

JAX 的 JSON 支持。

*   [JSON 示例用 Jersey+Jackson](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/json-example-with-jersey-jackson/)
    Jersey+Jackson 将对象映射到 JSON 和从 JSON 映射。
*   [JSON 示例用 rest easy+Jackson](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/integrate-jackson-with-resteasy/)
    rest easy+Jackson 将对象映射到 JSON 和从 JSON 映射。
*   [JSON 示例用 rest easy+JAXB+spoken](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/download-json-from-jax-rs-with-jaxb-resteasy/)
    rest easy+JAXB+spoken 将对象映射到 JSON 和从 JSON 映射。

## RESTful Java 客户端

创建一个 RESTful Java 客户端来执行“GET”和“POST”请求，以操作 json 数据。

*   [使用 java.net.URL 的 RESTful Java 客户端](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/restfull-java-client-with-java-net-url/)
*   [带 Apache HttpClient 的 RESTful Java 客户端](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/restful-java-client-with-apache-httpclient/)
*   [带有 RESTEasy 客户端的 RESTful Java 客户端](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/restful-java-client-with-resteasy-client-framework/)
*   [RESTful Java 客户端和 Jersey 客户端](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/restful-java-client-with-jersey-client/)

## JAX-遥感+春天

将 JAX 遥感系统与 Spring 框架集成。

*   [球衣+弹簧整合实例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jersey-spring-integration-example/)
    将球衣与弹簧框架整合。
*   [RESTEasy + Spring 集成实例](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/resteasy-spring-integration-example/)
    将 RESTEasy 与 Spring 框架集成。

## 常见错误消息

JAX 遥感器开发中的一些常见错误信息。

*   [RESTEasy 无法扫描 WEB-INF 中的 JAX-RS 注释，ZLIB 输入流意外结束](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/resteasy-unable-to-scan-web-inf-for-jax-rs-annotations/)
*   [ClassNotFoundException:org . JBoss . rest easy . plugins . providers . multipart . multipart input](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/classnotfoundexception-org-jboss-resteasy-plugins-providers-multipart-multipartinput/)
*   [rest easy–找不到类型为 multipart/form-data 的消息正文读取器](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/resteasy-could-not-find-message-body-reader-for-type-multipartform-data/)
*   [rest easy–找不到类型为 xx、媒体类型为 application/xml 的响应对象的 MessageBodyWriter】](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/resteasy-could-not-find-messagebodywriter-for-response-object-of-typexx-of-media-type-applicationxml/)
*   [将消息体注入到公共 org . codehaus . Jackson . jaxrs . jacksonjsonprovider 的单例中是非法的](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/illegal-to-inject-a-message-body-into-a-singleton-into-public-org-codehaus-jackson-jaxrs-jacksonjsonprovider/)
*   [Jersey:resource config 实例不包含任何根资源类](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/jersey-the-resourceconfig-instance-does-not-contain-any-root-resource-classes/)
*   [ClassNotFoundException:com . sun . jersey . SPI . container . servlet . servlet container](http://web.archive.org/web/20220706180444/http://www.mkyong.com/webservices/jax-rs/classnotfoundexception-com-sun-jersey-spi-container-servlet-servletcontainer/)

## 参考

1.  [球衣官网](http://web.archive.org/web/20220706180444/https://jersey.java.net/)
2.  [球衣用户指南](http://web.archive.org/web/20220706180444/https://jersey.java.net/nonav/documentation/latest/user-guide.html)
3.  [RESTEasy 官网](http://web.archive.org/web/20220706180444/http://www.jboss.org/resteasy)
4.  [RESTEasy 用户指南](http://web.archive.org/web/20220706180444/http://docs.jboss.org/resteasy/docs/2.2.1.GA/userguide/html/)
5.  [维客休息解释](http://web.archive.org/web/20220706180444/https://en.wikipedia.org/wiki/Java_API_for_RESTful_Web_Services)

<input type="hidden" id="mkyong-current-postId" value="9716">