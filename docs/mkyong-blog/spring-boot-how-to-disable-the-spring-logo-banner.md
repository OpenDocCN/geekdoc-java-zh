# Spring Boot——如何禁用 Spring 徽标横幅

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-how-to-disable-the-spring-logo-banner/>

以下是禁用 Spring 徽标横幅的几种方法:

```java
 .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.5.1.RELEASE) 
```

**Note**
You may interest in this – [Spring Boot – Custom Banner example](http://web.archive.org/web/20190214233233/http://www.mkyong.com/spring-boot/spring-boot-custom-banner-example/)

## 解决办法

1.`SpringApplication`主要方法。

SpringBootConsoleApplication.java

```java
 package com.mkyong;

import org.springframework.boot.Banner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBootConsoleApplication {

    public static void main(String[] args) throws Exception {

        SpringApplication app = new SpringApplication(SpringBootConsoleApplication.class);
        app.setBannerMode(Banner.Mode.OFF);
        app.run(args);

    }

} 
```

**Note**
There are 3 `Banner.Mode`

1.  关–禁止打印横幅。
2.  控制台–将横幅打印到 System.out。
3.  日志–将横幅打印到日志文件中。

2.应用程序属性文件。

application.properties

```java
 spring.main.banner-mode=off 
```

3.应用程序 YAML 文件。

application.yml

```java
 spring:
  main:
    banner-mode:"off" 
```

4.作为系统属性传递

Terminal

```java
 $ java -Dspring.main.banner-mode=off -jar spring-boot-simple-1.0.jar 
```

 ## 参考

1.  [Spring Boot–定制横幅示例](http://web.archive.org/web/20190214233233/http://www.mkyong.com/spring-boot/spring-boot-custom-banner-example/)
2.  [Spring Boot 横幅](http://web.archive.org/web/20190214233233/http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-banner)

[banner](http://web.archive.org/web/20190214233233/http://www.mkyong.com/tag/banner/) [spring boot](http://web.archive.org/web/20190214233233/http://www.mkyong.com/tag/spring-boot/)![](img/6def5f9774725ea3d3b9495417d8d5af.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214233233/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="14441">







