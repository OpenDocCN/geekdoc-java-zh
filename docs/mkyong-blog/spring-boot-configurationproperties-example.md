# Spring Boot @配置属性示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/>

![Spring Boot @ConfigurationProperties](img/e6f4b4474ca6bdf18ee17729f19ec484.png)

Spring Boot `@ConfigurationProperties`让开发者可以轻松地将整个`.properties`和`yml`文件映射成一个对象。

*PS 用 Spring Boot 2.1.2.RELEASE 测试*

## 1.@值

1.1 通常情况下，我们使用`@Value`来逐个注入`.properties`的值，这对于小且结构简单的`.properties`文件很好。举个例子，

global.properties

```java
 email=test@mkyong.com
thread-pool=12 
```

GlobalProperties.java

```java
 @Component
@PropertySource("classpath:global.properties")
public class GlobalProperties {

    @Value("${thread-pool}")
    private int threadPool;

    @Value("${email}")
    private String email;

    //getters and setters

} 
```

1.2`@ConfigurationProperties`中的等价物

GlobalProperties.java

```java
 import org.springframework.boot.context.properties.ConfigurationProperties;

@Component
@PropertySource("classpath:global.properties")
@ConfigurationProperties
public class GlobalProperties {

    private int threadPool;
    private String email;

    //getters and setters

} 
```

## 2.@配置属性

2.1 查看下面的复杂结构`.properties`或`yml`文件，我们如何通过`@Value`映射值？

application.properties

```java
 #Logging
logging.level.org.springframework.web=ERROR
logging.level.com.mkyong=DEBUG

#Global
email=test@mkyong.com
thread-pool=10

#App
app.menus[0].title=Home
app.menus[0].name=Home
app.menus[0].path=/
app.menus[1].title=Login
app.menus[1].name=Login
app.menus[1].path=/login

app.compiler.timeout=5
app.compiler.output-folder=/temp/

app.error=/error/ 
```

或 YAML 的同等机构。

application.yml

```java
 logging:
  level:
    org.springframework.web: ERROR
    com.mkyong: DEBUG
email: test@mkyong.com
thread-pool: 10
app:
  menus:
    - title: Home
      name: Home
      path: /
    - title: Login
      name: Login
      path: /login
  compiler:
    timeout: 5
    output-folder: /temp/
  error: /error/ 
```

**Note**
`@ConfigurationProperties` supports both `.properties` and `.yml` file.

2.2 `@ConfigurationProperties`前来救援:

AppProperties.java

```java
 package com.mkyong;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
@ConfigurationProperties("app") // prefix app, find app.* values
public class AppProperties {

    private String error;
    private List<Menu> menus = new ArrayList<>();
    private Compiler compiler = new Compiler();

    public static class Menu {
        private String name;
        private String path;
        private String title;

        //getters and setters

        @Override
        public String toString() {
            return "Menu{" +
                    "name='" + name + '\'' +
                    ", path='" + path + '\'' +
                    ", title='" + title + '\'' +
                    '}';
        }
    }

    public static class Compiler {
        private String timeout;
        private String outputFolder;

        //getters and setters

        @Override
        public String toString() {
            return "Compiler{" +
                    "timeout='" + timeout + '\'' +
                    ", outputFolder='" + outputFolder + '\'' +
                    '}';
        }

    }

    //getters and setters
} 
```

GlobalProperties.java

```java
 package com.mkyong;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties // no prefix, find root level values.
public class GlobalProperties {

    private int threadPool;
    private String email;

	//getters and setters
} 
```

## 3.@配置属性验证

这个`@ConfigurationProperties`支持 JSR-303 bean 验证。

3.1 在`@ConfigurationProperties`类上添加`@Validated`，在我们想要验证的字段上添加`javax.validation`注释。

GlobalProperties.java

```java
 package com.mkyong;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import org.springframework.validation.annotation.Validated;

import javax.validation.constraints.Max;
import javax.validation.constraints.Min;
import javax.validation.constraints.NotEmpty;

@Component
@ConfigurationProperties
@Validated
public class GlobalProperties {

    @Max(5)
    @Min(0)
    private int threadPool;

    @NotEmpty
    private String email;

    //getters and setters
} 
```

3.2 设置线程池=10

application.properties

```java
 #Global
email=test@mkyong.com
thread-pool=10 
```

3.3 启动 Spring Boot，我们将点击以下错误信息:

Console

```java
 ***************************
APPLICATION FAILED TO START
***************************

Description:

Binding to target org.springframework.boot.context.properties.bind.BindException: 
	Failed to bind properties under '' to com.mkyong.GlobalProperties failed:

    Property: .threadPool
    Value: 10
    Origin: class path resource [application.properties]:7:13
    Reason: must be less than or equal to 5

Action:

Update your application's configuration 
```

## 4.演示

```java
 $ git clone https://github.com/mkyong/spring-boot.git
$ cd externalize-config-properties-yaml
$ mvn spring-boot:run

access localhost:8080 
```

![demo](img/cf0c70a2c6ab3ee53e07e07f02acecdd.png)**Note**
For more detail, please refer to this official [Spring Boot Externalized Configuration](http://web.archive.org/web/20220815221140/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220815221140/https://github.com/mkyong/spring-boot.git)
$ cd externalize-config-properties-yaml
$ mvn spring-boot:run

访问本地主机:8080

## 参考

*   [Spring Boot–外部化配置](http://web.archive.org/web/20220815221140/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)
*   [Spring @PropertySource 示例](/web/20220815221140/https://mkyong.com/spring/spring-propertysources-example/)
*   [JSR 303: Bean 验证](http://web.archive.org/web/20220815221140/https://jcp.org/en/jsr/detail?id=303)

<input type="hidden" id="mkyong-current-postId" value="14267">