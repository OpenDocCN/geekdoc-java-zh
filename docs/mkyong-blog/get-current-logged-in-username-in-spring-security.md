# 在 Spring Security 中获取当前登录的用户名

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-security/get-current-logged-in-username-in-spring-security/>

在本文中，我们将向您展示在 Spring Security 中获取当前登录用户名的三种方法。

## 1.security context holder+authentic ation . getname()

```java
 import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class LoginController {

  @RequestMapping(value="/login", method = RequestMethod.GET)
  public String printUser(ModelMap model) {

      Authentication auth = SecurityContextHolder.getContext().getAuthentication();
      String name = auth.getName(); //get logged in username

      model.addAttribute("username", name);
      return "hello";

  }
  //... 
```

## 2.security context holder+user . get username()

```java
 import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class LoginController {

  @RequestMapping(value="/login", method = RequestMethod.GET)
  public String printUser(ModelMap model) {

      User user = (User)SecurityContextHolder.getContext().getAuthentication().getPrincipal();
      String name = user.getUsername(); //get logged in username

      model.addAttribute("username", name);
      return "hello";

  }
  //... 
```

## 3.usernamepasswordtauthenticationtoken

这是更优雅的解决方案，在运行时，Spring 会将`UsernamePasswordAuthenticationToken`注入到`Principal`接口中。

```java
 import java.security.Principal;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class LoginController {

  @RequestMapping(value="/login", method = RequestMethod.GET)
  public String printWelcome(ModelMap model, Principal principal ) {

      String name = principal.getName(); //get logged in username
      model.addAttribute("username", name);
      return "hello";

  }
  //... 
```

## 下载源代码

Download it – [Spring-Security-Get-Logged-In-Username.zip](http://web.archive.org/web/20201112024558/http://www.mkyong.com/wp-content/uploads/2011/08/Spring-Security-Get-Logged-In-Username.zip) (9 KB)

## 参考

1.  [安全上下文持有者 JavaDoc](http://web.archive.org/web/20201112024558/http://static.springsource.org/spring-security/site/docs/3.0.x/apidocs/org/springframework/security/core/context/SecurityContextHolder.html)
2.  [用户 JavaDoc](http://web.archive.org/web/20201112024558/http://static.springsource.org/spring-security/site/docs/3.0.x/apidocs/org/springframework/security/core/userdetails/User.html)
3.  [usernamepasswordtauthenticationtoken JavaDoc](http://web.archive.org/web/20201112024558/http://static.springsource.org/spring-security/site/docs/3.0.x/apidocs/org/springframework/security/authentication/UsernamePasswordAuthenticationToken.html)

Tags : [spring security](http://web.archive.org/web/20201112024558/https://mkyong.com/tag/spring-security/)<input type="hidden" id="mkyong-current-postId" value="10020">

### 相关文章

*   [春季安全教程](/web/20201112024558/https://www.mkyong.com/tutorials/spring-security-tutorials/)
*   [春日安全 hello world 示例](/web/20201112024558/https://www.mkyong.com/spring-security/spring-security-hello-world-example/)
*   [在 Spring Security 中显示自定义错误消息](/web/20201112024558/https://www.mkyong.com/spring-security/display-custom-error-message-in-spring-security/)
*   [Spring Security 自定义登录表单示例](/web/20201112024558/https://www.mkyong.com/spring-security/spring-security-form-login-example/)
*   [Spring Security HTTP 基本认证示例](/web/20201112024558/https://www.mkyong.com/spring-security/spring-security-http-basic-authentication-example/)

*   [Spring 安全密码哈希示例](/web/20201112024558/https://www.mkyong.com/spring-security/spring-security-password-hashing-example/)
*   [Spring 安全表单登录使用数据库](/web/20201112024558/https://www.mkyong.com/spring-security/spring-security-form-login-using-database/)
*   [ClassNotFoundException:DefaultSavedRequest](/web/20201112024558/https://www.mkyong.com/spring-security/classnotfoundexception-defaultsavedrequest/)
*   [Spring 安全访问控制示例](/web/20201112024558/https://www.mkyong.com/spring-security/spring-security-access-control-example/)
*   [Spring Security:自定义 403 拒绝访问页面](/web/20201112024558/https://www.mkyong.com/spring-security/customize-http-403-access-denied-page-in-spring-security/)