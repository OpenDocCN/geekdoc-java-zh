# spring Security–没有为 id“null”映射的 PasswordEncoder

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-security-there-is-no-passwordencoder-mapped-for-the-id-null/>

发送带有用户名和密码的 GET 请求，但遇到密码编码器错误？

测验

*   Spring Boot 2.1.2 .版本
*   发布

```java
 $ curl localhost:8080/books -u user:password

{
	"timestamp":"2019-02-22T15:03:49.322+0000",
	"status":500,
	"error":"Internal Server Error",
	"message":"There is no PasswordEncoder mapped for the id \"null\"",
	"path":"/books"
} 
```

日志中的错误

```java
 java.lang.IllegalArgumentException: There is no PasswordEncoder mapped for the id "null" 
```

下面是配置。

SpringSecurityConfig.java

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        auth.inMemoryAuthentication()
                .withUser("user").password("password").roles("USER")
                .and()
                .withUser("admin").password("password").roles("ADMIN");

    }

} 
```

## 解决办法

在 Spring Security 5.0 之前，默认的`PasswordEncoder`是`NoOpPasswordEncoder`，需要明文密码。在 Spring Security 5 中，默认为`DelegatingPasswordEncoder`，需要**密码存储格式**。

**读作[这个](http://web.archive.org/web/20221225035535/https://spring.io/blog/2017/11/01/spring-security-5-0-0-rc1-released#password-storage-format)和[这个](http://web.archive.org/web/20221225035535/https://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/#pe-dpe)和**

**解决方案 1**–添加密码存储格式，对于纯文本，添加`{noop}`

SpringSecurityConfig.java

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        auth.inMemoryAuthentication()
                .withUser("user").password("{noop}password").roles("USER")
                .and()
                .withUser("admin").password("{noop}password").roles("ADMIN");

    }

} 
```

**解决方案二**–`UserDetailsService`的`User.withDefaultPasswordEncoder()`

SpringSecurityConfig.java

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public UserDetailsService userDetailsService() {

        User.UserBuilder users = User.withDefaultPasswordEncoder();
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(users.username("user").password("password").roles("USER").build());
        manager.createUser(users.username("admin").password("password").roles("USER", "ADMIN").build());
        return manager;

    }

} 
```

## 参考

*   [Spring Security 5–密码存储格式](http://web.archive.org/web/20221225035535/https://spring.io/blog/2017/11/01/spring-security-5-0-0-rc1-released#password-storage-format)
*   [春季安全文档-授权密码编码](http://web.archive.org/web/20221225035535/https://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/#pe-dpe)

<input type="hidden" id="mkyong-current-postId" value="14930">