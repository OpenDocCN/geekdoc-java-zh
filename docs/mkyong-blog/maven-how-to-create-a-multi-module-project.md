# maven——如何创建多模块项目

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/maven-how-to-create-a-multi-module-project/>

![](img/f0c197b0ba3af0d02ffd5d7718a261ab.png)

在本教程中，我们将向您展示如何使用 Maven 来管理包含四个模块的多模块项目:

1.  密码模块–仅接口。
2.  密码 md5 模块–密码模块实现，MD5 密码散列。
3.  密码 sha 模块–密码模块实现，SHA 密码散列。
4.  Web 模块——一个简单的 MVC web 应用程序，使用 MD5 或 SHA 算法对输入进行哈希运算。

模块依赖性。

```java
 $ password 

$ password <-- password-md5

$ password <-- password-sha

$ web <-- (password-md5 | password-sha) <-- password 
```

构建多模块项目的一些命令，例如:

```java
 $ mvn -pl password compile	# compile password module only	

$ mvn -pl password-sha compile	# compile password-sha module, also dependency - password

$ mvn -pl web compile		# compile web module only	

$ mvn -am -pl web compile	# compile web module, also dependency - password-sha or password-md5, password

$ mvn -pl web jetty:run 	# run web module with Jetty

$ mvn compile 			# compile everything 
```

使用的技术:

1.  Maven 3.5.3
2.  JDK 8
3.  释放弹簧 5.1.0
4.  百里香叶

## 1.目录结构

在多模块项目布局中，一个父项目包含一个父`pom.xml`，每个子模块(子项目)也包含自己的`pom.xml`

![](img/f0c197b0ba3af0d02ffd5d7718a261ab.png)

## 2.Maven POM

让我们回顾一下 Maven 多模块 POM 文件。

2.1 一个父 POM 文件，打包为`pom`。

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

	<!-- parent pom -->
    <groupId>com.mkyong.multi</groupId>
    <artifactId>java-multi-modules</artifactId>
    <packaging>pom</packaging>
	<version>1.0</version>

	<!-- sub modules -->
	<modules>
        <module>web</module>
        <module>password</module>
        <module>password-sha</module>
        <module>password-md5</module>
    </modules>

</project> 
```

2.2 在密码模块中。

password/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">

	<!-- parent pom -->
    <parent>
        <artifactId>java-multi-modules</artifactId>
        <groupId>com.mkyong.multi</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

	<!-- password info -->
    <groupId>com.mkyong.password</groupId>
    <artifactId>password</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

</project> 
```

2.3 在密码 md5 模块中。

password-md5/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <parent>
        <artifactId>java-multi-modules</artifactId>
        <groupId>com.mkyong.multi</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

	<!-- password-md5 info -->
    <groupId>com.mkyong.password</groupId>
    <artifactId>password-md5</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>

</project> 
```

2.4 在密码-sha 模块中。

password-sha/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <parent>
        <artifactId>java-multi-modules</artifactId>
        <groupId>com.mkyong.multi</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

	<!-- password-sha info -->
    <groupId>com.mkyong.password</groupId>
    <artifactId>password-sha</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>

</project> 
```

2.4 在 web 模块中。

web/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!-- this is a parent pom -->
    <parent>
        <groupId>com.mkyong.multi</groupId>
        <artifactId>java-multi-modules</artifactId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <!-- web project info -->
    <groupId>com.mkyong</groupId>
    <artifactId>web</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>

    <dependencies>

		<!-- md5 or sha hashing -->

        <!-- md5 hash
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password-md5</artifactId>
            <version>1.0</version>
        </dependency>
        -->

        <!-- sha -->
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password-sha</artifactId>
            <version>1.0</version>
        </dependency>

    </dependencies>

</project> 
```

## 3.父项目

![](img/01bb1db64fcc3a1082888fe5076f6a0b.png)

3.1 父 pom，那些属性和依赖(JDK 8，JUnit 5，Spring 5)被所有子模块共享。

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mkyong.multi</groupId>
    <artifactId>java-multi-modules</artifactId>
    <packaging>pom</packaging>
    <version>1.0</version>

    <properties>
        <!-- https://maven.apache.org/general.html#encoding-warning -->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>5.3.1</junit.version>
        <spring.version>5.1.0.RELEASE</spring.version>
    </properties>

    <modules>
        <module>web</module>
        <module>password</module>
        <module>password-sha</module>
        <module>password-md5</module>
    </modules>

    <dependencies>

        <!-- Spring DI for all modules -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- unit test -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-params</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.0</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <version>3.1.1</version>
            </plugin>
        </plugins>
    </build>
</project> 
```

## 4.密码模块

4.1 仅包含一个接口的模块。

![](img/ec93c38d2ce1b551ed0679223bd14225.png)

4.2 POM 文件。

password/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>java-multi-modules</artifactId>
        <groupId>com.mkyong.multi</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mkyong.password</groupId>
    <artifactId>password</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

</project> 
```

4.3 一个接口。

PasswordService.java

```java
 package com.mkyong.password;

public interface PasswordService {

    String hash(String input);

    String algorithm();

} 
```

## 5.密码-md5 模块

5.1 密码模块实现。

![](img/be03d907dd77cf7c9f8bfc5db7d204ac.png)

5.2 POM 文件。

password-md5/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>java-multi-modules</artifactId>
        <groupId>com.mkyong.multi</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <!-- password-md5 info -->
    <groupId>com.mkyong.password</groupId>
    <artifactId>password-md5</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>

</project> 
```

5.3 MD5 散列法。

PasswordServiceImpl.java

```java
 package com.mkyong.password;

import org.springframework.stereotype.Service;

import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

@Service
public class PasswordServiceImpl implements PasswordService {

    @Override
    public String hash(String input) {
        return md5(input);
    }

    @Override
    public String algorithm() {
        return "md5";
    }

    private String md5(String input) {

        StringBuilder result = new StringBuilder();
        MessageDigest md;

        try {
            md = MessageDigest.getInstance("MD5");
            byte[] hashInBytes = md.digest(input.getBytes(StandardCharsets.UTF_8));

            for (byte b : hashInBytes) {
                result.append(String.format("%02x", b));
            }
        } catch (NoSuchAlgorithmException e) {
            throw new IllegalArgumentException(e);
        }

        return result.toString();
    }
} 
```

5.4 单元测试。

TestPasswordService.java

```java
 package com.mkyong.password;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class TestPasswordService {

    PasswordService passwordService;

    @BeforeEach
    void init() {
        passwordService = new PasswordServiceImpl();
    }

    @DisplayName("md5 -> hex")
    @ParameterizedTest
    @CsvSource({
            "123456, e10adc3949ba59abbe56e057f20f883e",
            "hello world, 5eb63bbbe01eeed093cb22bb8f5acdc3"
    })
    void testMd5hex(String input, String expected) {
        assertEquals(expected, passwordService.hash(input));
    }

} 
```

## 6.密码-sha 模块

6.1 密码模块实现。

![](img/ff09fce11eac3ecdac1a1ad86df25afa.png)

6.2 POM 文件。

password-sha/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>java-multi-modules</artifactId>
        <groupId>com.mkyong.multi</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mkyong.password</groupId>
    <artifactId>password-sha</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>

    <properties>
        <commos.codec.version>1.11</commos.codec.version>
    </properties>

    <dependencies>

        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>${commos.codec.version}</version>
        </dependency>

        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password</artifactId>
            <version>1.0</version>
        </dependency>

    </dependencies>

</project> 
```

6.3 SHA 与 [Apache 通用编解码器库](http://web.archive.org/web/20211129035048/https://commons.apache.org/proper/commons-codec/)进行哈希运算

PasswordServiceImpl.java

```java
 package com.mkyong.password;

import org.apache.commons.codec.digest.DigestUtils;
import org.springframework.stereotype.Service;

@Service
public class PasswordServiceImpl implements PasswordService {

    @Override
    public String hash(String input) {
        return DigestUtils.sha256Hex(input);
    }

    @Override
    public String algorithm() {
        return "sha256";
    }

} 
```

6.4 单元测试。

TestPasswordService.java

```java
 package com.mkyong.password;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class TestPasswordService {

    PasswordService passwordService;

    @BeforeEach
    void init() {
        passwordService = new PasswordServiceImpl();
    }

    @DisplayName("sha256 -> hex")
    @ParameterizedTest
    @CsvSource({
            "123456, 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92",
            "hello world, b94d27b9934d3e08a52e52d7da7dabfac484efe37a5380ee9088f7ace2efcde9"
    })
    void testSha256hex(String input, String expected) {
        assertEquals(expected, passwordService.hash(input));
    }

} 
```

## 7.Web 模块

7.1 一个 Spring MCV +百里香网络应用程序，用 md5 或 sha 算法散列一个输入。

![](img/2f7b9c585a56f49d824941064a479fe2.png)

7.2 POM 文件。

web/pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!-- this is a parent pom -->
    <parent>
        <groupId>com.mkyong.multi</groupId>
        <artifactId>java-multi-modules</artifactId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <!-- web project info -->
    <groupId>com.mkyong</groupId>
    <artifactId>web</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>

    <properties>
        <thymeleaf.version>3.0.10.RELEASE</thymeleaf.version>
    </properties>

    <dependencies>

        <!-- md5 hash
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password-md5</artifactId>
            <version>1.0</version>
        </dependency>
        -->

        <!-- sha -->
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password-sha</artifactId>
            <version>1.0</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- logging , spring 5 no more bridge, thanks spring-jcl -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>

        <!-- need this for unit test -->
        <dependency>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-library</artifactId>
            <version>1.3</version>
            <scope>test</scope>
        </dependency>

        <!-- servlet api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>

        <!-- thymeleaf view -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf</artifactId>
            <version>${thymeleaf.version}</version>
        </dependency>

        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>${thymeleaf.version}</version>
        </dependency>

    </dependencies>

    <build>
        <finalName>java-web-project</finalName>
        <plugins>

            <plugin>
                <groupId>org.eclipse.jetty</groupId>
                <artifactId>jetty-maven-plugin</artifactId>
                <version>9.4.12.v20180830</version>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.2</version>
            </plugin>

        </plugins>
    </build>

</project> 
```

7.3 Web 模块依赖性

Terminal

```java
 $ mvn -pl web dependency:tree

[INFO] com.mkyong:web:war:1.0
[INFO] +- com.mkyong.password:password-sha:jar:1.0:compile
[INFO] |  +- commons-codec:commons-codec:jar:1.11:compile
[INFO] |  \- com.mkyong.password:password:jar:1.0:compile
[INFO] +- org.springframework:spring-webmvc:jar:5.1.0.RELEASE:compile
[INFO] |  +- org.springframework:spring-aop:jar:5.1.0.RELEASE:compile
[INFO] |  +- org.springframework:spring-beans:jar:5.1.0.RELEASE:compile
[INFO] |  +- org.springframework:spring-core:jar:5.1.0.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-jcl:jar:5.1.0.RELEASE:compile
[INFO] |  +- org.springframework:spring-expression:jar:5.1.0.RELEASE:compile
[INFO] |  \- org.springframework:spring-web:jar:5.1.0.RELEASE:compile
[INFO] +- org.springframework:spring-test:jar:5.1.0.RELEASE:compile
[INFO] +- ch.qos.logback:logback-classic:jar:1.2.3:compile
[INFO] |  +- ch.qos.logback:logback-core:jar:1.2.3:compile
[INFO] |  \- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO] +- org.hamcrest:hamcrest-library:jar:1.3:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] +- javax.servlet:javax.servlet-api:jar:3.1.0:provided
[INFO] +- org.thymeleaf:thymeleaf:jar:3.0.10.RELEASE:compile
[INFO] |  +- ognl:ognl:jar:3.1.12:compile
[INFO] |  |  \- org.javassist:javassist:jar:3.20.0-GA:compile
[INFO] |  +- org.attoparser:attoparser:jar:2.0.5.RELEASE:compile
[INFO] |  \- org.unbescape:unbescape:jar:1.1.6.RELEASE:compile
[INFO] +- org.thymeleaf:thymeleaf-spring5:jar:3.0.10.RELEASE:compile
[INFO] +- org.springframework:spring-context:jar:5.1.0.RELEASE:compile
[INFO] +- org.junit.jupiter:junit-jupiter-engine:jar:5.3.1:test
[INFO] |  +- org.apiguardian:apiguardian-api:jar:1.0.0:test
[INFO] |  +- org.junit.platform:junit-platform-engine:jar:1.3.1:test
[INFO] |  |  +- org.junit.platform:junit-platform-commons:jar:1.3.1:test
[INFO] |  |  \- org.opentest4j:opentest4j:jar:1.1.1:test
[INFO] |  \- org.junit.jupiter:junit-jupiter-api:jar:5.3.1:test
[INFO] \- org.junit.jupiter:junit-jupiter-params:jar:5.3.1:test
[INFO] ------------------------------------------------------------------------ 
```

7.4 春天的东西，整合百里香叶和配置。

SpringConfig.java

```java
 package com.mkyong.web.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.thymeleaf.spring5.SpringTemplateEngine;
import org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver;
import org.thymeleaf.spring5.view.ThymeleafViewResolver;
import org.thymeleaf.templatemode.TemplateMode;

@EnableWebMvc
@Configuration
@ComponentScan({"com.mkyong"})
public class SpringConfig implements WebMvcConfigurer {

    @Autowired
    private ApplicationContext applicationContext;

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/resources/**")
                .addResourceLocations("/resources/");
    }

    @Bean
    public SpringResourceTemplateResolver templateResolver() {
        SpringResourceTemplateResolver templateResolver = new SpringResourceTemplateResolver();
        templateResolver.setApplicationContext(this.applicationContext);
        templateResolver.setPrefix("/WEB-INF/views/");
        templateResolver.setSuffix(".html");
        templateResolver.setTemplateMode(TemplateMode.HTML);
        templateResolver.setCacheable(true);
        return templateResolver;
    }

    @Bean
    public SpringTemplateEngine templateEngine() {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver());
        templateEngine.setEnableSpringELCompiler(true);
        return templateEngine;
    }

    @Bean
    public ThymeleafViewResolver viewResolver() {
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
        viewResolver.setTemplateEngine(templateEngine());
        return viewResolver;
    }

} 
```

WebInitializer.java

```java
 package com.mkyong.web;

import com.mkyong.web.config.SpringConfig;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class WebInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null;
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

} 
```

WelcomeController.java

```java
 package com.mkyong.web.controller;

import com.mkyong.password.PasswordService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class WelcomeController {

    private final Logger logger = LoggerFactory.getLogger(WelcomeController.class);

    @Autowired
    private PasswordService passwordService;

    @GetMapping("/")
    public String welcome(@RequestParam(name = "query",
            required = false, defaultValue = "123456") String query, Model model) {

        logger.debug("Welcome to mkyong.com... Query : {}", query);

        model.addAttribute("query", query);
        model.addAttribute("hash", passwordService.hash(query));
        model.addAttribute("algorithm", passwordService.algorithm());

        return "index";
    }

} 
```

7.5 Spring MVC 的单元测试。

TestWelcome.java

```java
 package com.mkyong.web;

import com.mkyong.web.config.SpringConfig;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.junit.jupiter.web.SpringJUnitWebConfig;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringJUnitWebConfig(SpringConfig.class)
@DisplayName("Test Spring MVC default view")
public class TestWelcome {

    private MockMvc mockMvc;

    @Autowired
    private WebApplicationContext webAppContext;

    @BeforeEach
    public void setup() {
        mockMvc = MockMvcBuilders.webAppContextSetup(webAppContext).build();
    }

    @Test
    public void testDefault() throws Exception {

        this.mockMvc.perform(
                get("/"))
                .andExpect(status().isOk())
                .andExpect(view().name("index"))
                .andExpect(model().attribute("query", "123456"));
        //.andExpect(model().attribute("sha256", "8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92"))
        //.andExpect(model().attribute("md5", "e10adc3949ba59abbe56e057f20f883e"));

    }

} 
```

七点六分。

index.html

```java
 <!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<h1 th:text="'Input : ' + ${query}"/>
<h1 th:text="'Algorithm : ' + ${algorithm}"/>
<h3 th:text="${hash}" />
</body>
</html> 
```

完成了。

## 8.演示

8.1 在父项目中，使用标准的`mvn compile`来编译所有的模块，Maven 将决定编译顺序。

Terminal

```java
 $ mvn compile 

[INFO] Reactor Summary:
[INFO]
[INFO] java-multi-modules 1.0 ............................. SUCCESS [  0.007 s]
[INFO] password ........................................... SUCCESS [  0.539 s]
[INFO] password-sha ....................................... SUCCESS [  0.038 s]
[INFO] web ................................................ SUCCESS [  0.080 s]
[INFO] password-md5 1.0 ................................... SUCCESS [  0.035 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.822 s
[INFO] Finished at: 2018-10-23T15:32:21+08:00
[INFO] ------------------------------------------------------------------------ 
```

8.2 将所有模块安装到本地存储库中。

Terminal

```java
 $ mvn install 
```

8.3 只编译 web 模块(Maven 将从本地存储库中找到密码模块依赖项，这就是为什么我们需要`mvn install`

Terminal

```java
 $ mvn -pl web compile 
```

8.4 或者，添加`-am`来编译 web 模块及其依赖模块(密码-sha 和密码)。

Terminal

```java
 $ mvn -am -pl web compile

[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] java-multi-modules                                                 [pom]
[INFO] password                                                           [jar]
[INFO] password-sha                                                       [jar]
[INFO] web                                                                [war]

......

[INFO] Reactor Summary:
[INFO]
[INFO] java-multi-modules 1.0 ............................. SUCCESS [  0.009 s]
[INFO] password ........................................... SUCCESS [  0.534 s]
[INFO] password-sha ....................................... SUCCESS [  0.036 s]
[INFO] web 1.0 ............................................ SUCCESS [  0.077 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.780 s
[INFO] Finished at: 2018-10-23T15:36:16+08:00
[INFO] ------------------------------------------------------------------------ 
```

8.5 用`mvn -pl web jetty:run`命令测试网络模块。

Terminal

```java
 $ mvn install

$ mvn -pl web jetty:run

//...
[INFO] 1 Spring WebApplicationInitializers detected on classpath
[INFO] DefaultSessionIdManager workerName=node0
[INFO] No SessionScavenger set, using defaults
[INFO] node0 Scavenging every 600000ms
[INFO] Started ServerConnector@40a8a26f{HTTP/1.1,[http/1.1]}{0.0.0.0:8080}
[INFO] Started @6398ms
[INFO] Started Jetty Server 
```

8.6*http://localhost:8080*

![](img/6b9377b2751019a1bd32b8a5ae042554.png)

8.7 *http://localhost:8080/？query=mkyong*

![](img/32afe849409ec747fe4e4d8246dce26b.png)

8.8 MD5 算法更新。重启 Jetty 服务器。

web/pom.xml

```java
 <!-- md5 hash -->
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password-md5</artifactId>
            <version>1.0</version>
        </dependency>

        <!-- sha
        <dependency>
            <groupId>com.mkyong.password</groupId>
            <artifactId>password-sha</artifactId>
            <version>1.0</version>
        </dependency>
        --> 
```

8.9 *http://localhost:8080/？query=mkyong*

![](img/be8b79ed3a0bf625246c66700ac2fb4f.png)

## 下载源代码

$ git clone [https://github.com/mkyong/maven-examples.git](http://web.archive.org/web/20211129035048/https://github.com/mkyong/maven-examples.git)
$ cd java-multi-modules
$ mvn install
$ mvn -pl web jetty:run

## 参考

1.  [教程:百里香叶+春天](http://web.archive.org/web/20211129035048/https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html)
2.  [使用 Spring MVC 提供 Web 内容](http://web.archive.org/web/20211129035048/https://spring.io/guides/gs/serving-web-content/)
3.  [多模块项目](http://web.archive.org/web/20211129035048/https://books.sonatype.com/mvnex-book/reference/multimodule.html)
4.  [Spring IO–创建多模块项目](http://web.archive.org/web/20211129035048/https://spring.io/guides/gs/multi-module/)
5.  [使用 Maven 和 Gradle 构建多模块项目](http://web.archive.org/web/20211129035048/http://andresalmiray.com/multi-module-project-builds-with-maven-and-gradle/)
6.  [Maven——如何创建一个 Java 项目](http://web.archive.org/web/20211129035048/http://www.mkyong.com/maven/how-to-create-a-java-project-with-maven/)
7.  [Maven–如何创建 Java web 应用项目](http://web.archive.org/web/20211129035048/http://www.mkyong.com/maven/how-to-create-a-web-application-project-with-maven/)

<input type="hidden" id="mkyong-current-postId" value="14758">