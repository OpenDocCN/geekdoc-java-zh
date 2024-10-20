# Spring MVC 教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/spring-mvc-tutorials/>

![Spring MVC Tutorials](img/c4dff36b4ecd07e1b877f83a9ff187a3.png "Spring MVC Tutorials")

[Spring MVC](http://web.archive.org/web/20220803170141/https://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc) ，一个 Java **模型-视图-控制器(MVC)** web 框架，它建立在 Spring 反转控制(IoC)框架之上。

**Rewrite and Spring 4 (12/Jun/2015)**
I’m rewriting the outdated articles and upgrade it to Spring 4, give me some time 🙂

## 1.Spring MVC Hello World

快速启动 Spring MVC 框架的一些 hello world 示例。

*   [Gradle–Spring 4 MVC Hello World 示例(XML 配置)](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/gradle-spring-mvc-web-project-example/)
*   [Gradle–Spring 4 MVC Hello World 示例(@JavaConfig + Servlet 3)](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/gradle-spring-4-mvc-hello-world-example-annotation/)
*   [Maven–Spring 3 MVC Hello World 示例(XML 配置)](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring3/spring-3-mvc-hello-world-example/)
*   [Maven–Spring 3 MVC Hello World 示例(@JavaConfig + Servlet 3)](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring3/spring-3-mvc-hello-world-example-annotation/)
*   弹簧 3 静止示例(工作)
*   弹簧 4 静止示例(工作)
*   [`@Deprecated`–Maven+Spring 2 . 5 . 6 MVC hello world 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-hello-world-example/)
*   [`@Deprecated`–Maven+Spring 2 . 5 . 6 MVC hello world 注释示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-hello-world-annotation-example/)

**新&更新……**

*   [Spring 4 MVC Ajax 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-4-mvc-ajax-hello-world-example/)
*   [弹簧 4 表单处理示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-example/)
*   [Spring 4 + Logback 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-logback-slf4j-example/)
*   …

## 2.处理程序映射

定义 web 请求(URL)如何映射到控制器处理程序。

*   [BeanNameUrlHandlerMapping 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-beannameurlhandlermapping-example/)
    将请求的 URL 映射到 cocntroller 的名称。
*   [ControllerClassNameHandlerMapping 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-controllerclassnamehandlermapping-example/)
    使用约定将请求的 URL 映射到控制器。
*   [SimpleUrlHandlerMapping 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-simpleurlhandlermapping-example/)
    允许开发者明确指定 URL 模式的映射和处理程序映射。
*   [配置处理程序映射优先级](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/configure-the-handler-mapping-priority-in-spring-mvc/)
    如果应用了多个视图处理程序映射，您必须声明优先级以避免冲突问题。

## 3.控制器

控制器类来处理 web 请求。

*   [MultiActionController 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-example/)
    将相关动作分组到一个控制器类中。
*   [MultiActionController 注释示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-multiactioncontroller-annotation-example/)
    多动作控制器通过使用注释，@RequestMapping，他得到了最佳且简单的解决方案。
*   [properties methodnameresolver 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-propertiesmethodnameresolver-example/)
    multi action controller 控制器类的一个灵活的方法名解析器，它允许显式定义请求的 URL 和方法名之间的映射。
*   [ParameterMethodNameResolver 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-parametermethodnameresolver-example/)
    multi action controller 控制器类的另一个方法名解析器，它允许通过请求的参数名将 URL 映射到方法名。
*   [ParameterizableViewController 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-parameterizableviewcontroller-example/)
    有了 ParameterizableViewController，就不再需要在控制器类中硬编码视图名，而是通过 ParameterizableViewController 的“view name”属性来指定视图名。

## 4.查看解析程序

将从控制器类返回的“视图名称”解析为物理视图页面或 JSP 页面。

*   [InternalResourceViewResolver 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-internalresourceviewresolver-example/)
    给视图名添加预定义的前缀和后缀(前缀+视图名+后缀)，并生成最终的视图页面 URL。
*   [XmlViewResolver 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-xmlviewresolver-example/)
    将视图 beans 放入 XML 文件中。
*   [ResourceBundleViewResolver 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-resourcebundleviewresolver-example/)
    将视图 beans 放入”。属性”文件。
*   [配置多视图解析器优先级](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/configure-multiple-view-resolvers-priority-in-spring-mvc/)
    如果应用了多视图解析器策略，您必须声明优先级以避免冲突问题。

## 5.表单处理

Spring MVC 中的表单处理。

*   [表单处理示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-example/)
    Spring MVC 中的表单处理，基于 XML 的版本。
*   [表单处理注释示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-form-handling-annotation-example/)
    Spring MVC 中的表单处理，注释版本。
*   [处理重复表单提交](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/handling-duplicate-form-submission-in-spring-mvc/)
    “Post/Redirect/Get”设计模式是这种重复表单提交问题的常用解决方案。
*   [RedirectView 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-redirectview-example/)
    重定向到另一个绝对、上下文相对或当前请求相对 URL 的视图。
*   [处理多页/向导窗体](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-handling-multipage-forms-with-abstractwizardformcontroller/)
    如何处理多页窗体或向导窗体。

## 6.Spring 的表单标签库

通过 Spring 的 form 标签呈现 HTML 表单的组件。

*   [Textbox 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-textbox-example/)
    <表单:输入/ >标签，呈现一个 HTML textbox 字段。
*   [密码示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-password-example/)
    <表单:password / >标签，呈现一个 HTML 密码字段。
*   [Textarea 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-textarea-example/)
    <表单:textarea / >标签，呈现一个 HTML 的 Textarea 字段。
*   [复选框和复选框示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-checkbox-and-checkboxes-example/)
    <表单:复选框/ >，<表单:复选框/ >标签，呈现单个或多个 HTML 复选框。
*   [Radiobutton 和 Radiobutton 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-radiobutton-and-radiobuttons-example/)
    <表单:radiobutton / >，<表单:radio button/>标签，呈现单个或多个 HTML 单选按钮。
*   [下拉列表框示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-dropdown-box-example/)
    <表单:select / >，<表单:option / >和<表单:options / >标签，呈现一个 HTML 下拉框、多选框和 list box。
*   [隐藏值示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-hidden-value-example/)
    <表单:hidden / >标签，呈现一个 HTML 隐藏值字段。
*   [Form errors 标签示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-form-errors-tag-example/)
    < form:errors / >标签，呈现那些表单组件的错误消息。
*   [文件上传示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-file-upload-example/)
    通过 Spring MultipartResolver 进行文件上传处理。

## 7.将 Spring MVC 与其他框架集成

将 Spring MVC 与其他集成在一起。

*   [Spring 3 MVC 和 JSR303 @Valid 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-jsr303-valid-example/)
    使用 Hibernate validator (JSR303 实现)来验证 Spring MVC 中的 bean。
*   [Spring 3 MVC 和 RSS 提要示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-rss-feed-example/)
    使用 ROME 从 Spring MVC 生成 RSS 提要。
*   [Spring 3 MVC 和 XML 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-xml-example/)
    使用 JAXB 从 Spring MVC 生成 XML 输出。
*   [Spring 3 MVC 和 JSON 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-3-mvc-and-json-example/)
    使用 Jackson 从 Spring MVC 生成 JSON 输出。
*   [通过 AbstractExcelView](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-export-data-to-excel-file-via-abstractexcelview/)
    Spring MVC 和 Excel 文件使用 AbstractExcelView 通过 Apache POI 库将数据导出到 Excel 文件。
*   [通过 AbstractJExcelView](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-export-data-to-excel-file-via-abstractjexcelview/)
    Spring MVC 和 Excel 文件使用 AbstractJExcelView 通过 JExcelAPI 库将数据导出到 Excel 文件。
*   [通过 AbstractPdfView](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-export-data-to-pdf-file-via-abstractpdfview/)
    Spring MVC 和 Pdf 文件使用 abstract PDF view 通过 Bruno Lowagie 的 iText 库将数据导出为 PDF 文件。
*   [Spring MVC 与 Log4j 集成示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-log4j-integration-example/)
    将 Log4j 集成到 Spring MVC 应用程序中的示例。

## 8.常见问题

一些常见的使用案例。

*   [Spring 3 MVC contentnegotiatingviewrolver 示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-3-mvc-contentnegotiatingviewresolver-example/)
    这个视图解析器让您根据重复请求文件扩展名来确定将返回哪个视图解析器。
*   [异常处理示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-exception-handling-example/)
    Spring MVC 中的异常处理。
*   [处理程序拦截器示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-handler-interceptors-example/)
    通过处理程序拦截器拦截 web 请求。
*   [国际化示例](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-internationalization-example/)
    Spring MVC 支持多种语言。

## 10.常见错误

Spring MVC 常见错误及解决方案。

*   [ModelAndView 的模型值没有通过 el 显示在 JSP 中](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/modelandviews-model-value-is-not-displayed-in-jsp-via-el/)
*   [404 错误代码在 Spring MVC 中不起作用](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/404-error-code-is-not-working-in-spring-mvc/)
*   [无法更改 HTTP accept 头–使用不同的区域设置解析策略](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/cannot-change-http-accept-header-use-a-different-locale-resolution-strategy/)
*   [Spring MVC 未能转换文件上传表单中的属性值](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/spring-mvc-failed-to-convert-property-value-in-file-upload-form/)
*   【bean 名称“xxx”的 BindingResult 和 plain target 对象都不能作为请求属性
*   [ClassNotFoundException:com . sun . syndication . feed . wire feed](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/classnotfoundexception-com-sun-syndication-feed-wirefeed/)
*   [ClassNotFoundException:com . thoughtworks . xstream . io . hierarchical streamreader](http://web.archive.org/web/20220803170141/http://www.mkyong.com/spring-mvc/classnotfoundexception-com-thoughtworks-xstream-io-hierarchicalstreamreader/)

## Spring Web MVC 参考

1.  [Spring 4.2.x MVC 文档](http://web.archive.org/web/20220803170141/https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/spring-web.html)
2.  [Spring 3.2.x MVC 文档](http://web.archive.org/web/20220803170141/https://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/spring-web.html)
3.  [Spring 2.5.6 MVC 文档](http://web.archive.org/web/20220803170141/https://docs.spring.io/spring/docs/2.5.6/reference/spring-web.html)

<input type="hidden" id="mkyong-current-postId" value="6810">