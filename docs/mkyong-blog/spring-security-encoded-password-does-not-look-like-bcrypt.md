# Spring Security:编码的密码看起来不像 BCrypt

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-security/spring-security-encoded-password-does-not-look-like-bcrypt/>

在 Spring Security 中，使用 *bcrypt* 密码散列的数据库认证。

```java
  import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
  import org.springframework.security.crypto.password.PasswordEncoder;
  //...
	String password = "123456";
	PasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
	String hashedPassword = passwordEncoder.encode(password);

```

spring-security.xml

```java
 <authentication-manager>
	<authentication-provider>
	    <password-encoder hash="bcrypt" />
	    //...
	</authentication-provider>
  </authentication-manager> 
```

```java
 CREATE  TABLE users (
  username VARCHAR(45) NOT NULL ,
  password VARCHAR(45) NOT NULL ,
  enabled TINYINT NOT NULL DEFAULT 1 ,
  PRIMARY KEY (username)); 
```

查看调试输出，总是说“**编码的密码看起来不像 BCrypt** ”，即使提供了正确的密码。

```java
 //...
12:56:31.868 DEBUG o.s.jdbc.datasource.DataSourceUtils - Returning JDBC Connection to DataSource
12:56:31.868 WARN  o.s.s.c.bcrypt.BCryptPasswordEncoder - Encoded password does not look like BCrypt
12:56:31.868 DEBUG o.s.s.a.d.DaoAuthenticationProvider - Authentication failed: password does not match stored value 
```

## 解决办法

例如，在 *bcrypt* 散列算法中，每次都会生成长度为 60 的不同散列值

```java
 $2a$10$LOqePml/koRGsk2YAIOFI.1YNKZg7EsQ5BAIuYP1nWOyYRl21dlne 
```

一个常见的错误，“密码”列(users 表)的长度小于 60，比如`password VARCHAR(45)`，有些数据库会自动截断数据。因此，您总是得到警告“编码的密码看起来不像 BCrypt”。

**要解决**，请确保“密码”栏的长度至少为 60。

[bcryt](http://web.archive.org/web/20190309053253/http://www.mkyong.com/tag/bcryt/) [hashing](http://web.archive.org/web/20190309053253/http://www.mkyong.com/tag/hashing/) [spring security](http://web.archive.org/web/20190309053253/http://www.mkyong.com/tag/spring-security/)![](img/1e73a701955db087f1b50d4f05f654f8.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190309053253/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="13325">







