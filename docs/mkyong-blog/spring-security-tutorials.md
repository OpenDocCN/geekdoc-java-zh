# 春季安全教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/spring-security-tutorials/>

![spring security tutorials](img/e56eae4c549197535925a3faca88d3d1.png)

Spring Security 是一个灵活而强大的认证和访问控制框架，用于保护基于 Spring 的 Java web 应用程序。

本教程中使用的 Spring 版本:

1.  弹簧 3.2.8 释放
2.  Spring Security 3.2.3 .发布

**Note**
Try this [Spring Boot + Spring Security + Thymeleaf example](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-boot/spring-boot-spring-security-thymeleaf-example/)

## 1.Spring 安全示例

向您展示如何使用 Spring Security 保护您的 web 应用程序的示例。

*   [Spring Security Hello World XML 示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-hello-world-example/)
    Spring MVC+Spring Security 基于 XML 的项目，使用默认登录表单。
*   [Spring Security Hello World 注释示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-hello-world-annotation-example/)
    Spring MVC+Spring Security 基于注释的项目，使用默认登录表单。
*   [Spring Security 自定义登录表单 XML 示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-form-login-example/)
    Spring MVC+Spring Security 基于 XML 的项目，自定义登录表单，注销功能，CSRF 保护和内存认证。
*   [Spring Security 自定义登录表单注释示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-custom-login-form-annotation-example/)
    Spring MVC+Spring Security 基于注释的项目，自定义登录表单，注销功能，CSRF 保护和内存认证。
*   [使用数据库的 Spring 安全表单登录–XML 和注释示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-form-login-using-database/)
    数据库认证、Spring 安全、JSP taglibs、JDBC、定制 403 拒绝访问页面等，XML 和注释均有。
*   [Spring Security:限制登录尝试——XML 和注释示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-limit-login-attempts-example/)
    如果用户尝试了 3 次无效登录，则锁定用户帐户。
*   [春安记我的例子](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-remember-me-example/) 【T2 记我的“持久令牌方法”的例子。额外:来自“记住我”cookie 的用户登录无法执行更新操作。
*   [Spring Security 密码哈希示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-password-hashing-example/)
    密码编码器采用 BCrypt 算法。
*   [Spring Security + Hibernate XML 示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-hibernate-xml-example/)
    使用 Hibernate 加载用户进行数据库认证。
*   [Spring Security + Hibernate 注释示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-hibernate-annotation-example/)
    使用 Hibernate 加载用户进行数据库认证。

## 2.常见问题

春季安全中的一些常见问题。

*   [定制 403 拒绝访问页面](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/customize-http-403-access-denied-page-in-spring-security/)
    像主题一样，向你展示了如何定制一个 403 拒绝访问页面，使用错误页面属性和定制处理程序。
*   [检查用户是否来自“记住我”cookie](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-check-if-user-is-from-remember-me-cookie/)
    如果认证= = RememberMeAuthenticationToken
*   [编码的密码看起来不像 BCrypt](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-encoded-password-does-not-look-like-bcrypt/)
    “密码”的长度不足以存储 BCrypt 的散列值。
*   [如何在 Spring Security 中获取当前登录用户名](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/get-current-logged-in-username-in-spring-security/)
    在 Spring Security 中获取当前登录用户名的 3 种方法。
*   [ClassNotFoundException:org . spring framework . security . web . saved request . defaultsavedrequest](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/classnotfoundexception-defaultsavedrequest/)

一些过时或废弃的文章…可能会在未来更新。

*   [Spring Security HTTP 基本认证示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-http-basic-authentication-example/)
    浏览器显示登录对话框进行认证。
*   [Spring 安全访问控制示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-access-control-example/)
    在 web 应用上实现访问控制或授权的示例。
*   [在 Spring Security 中显示自定义错误消息](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/display-custom-error-message-in-spring-security/)
    如何轻松覆盖默认的 Spring Security 错误消息。
*   [Spring Security 注销示例](http://web.archive.org/web/20221207085341/http://www.mkyong.com/spring-security/spring-security-logout-example/)
    简单的示例向您展示如何实现注销功能。

## 参考

1.  [春安官方页面](http://web.archive.org/web/20221207085341/https://projects.spring.io/spring-security/)
2.  [Spring Security 3.2.x 参考](http://web.archive.org/web/20221207085341/https://docs.spring.io/spring-security/site/docs/3.2.x/reference/htmlsingle/)
3.  [使用 Spring Security 进行自定义认证](http://web.archive.org/web/20221207085341/https://beansgocrazy.blogspot.com/2011/07/custom-authentication-with-spring.html)

<input type="hidden" id="mkyong-current-postId" value="10098">