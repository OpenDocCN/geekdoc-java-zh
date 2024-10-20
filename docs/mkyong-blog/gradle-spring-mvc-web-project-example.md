# Spring MVC hello world 示例(Gradle 和 JSP)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/gradle-spring-mvc-web-project-example/>

本教程向您展示了如何使用 [Jakarta 服务器页面](http://web.archive.org/web/20221118095838/https://en.wikipedia.org/wiki/Jakarta_Server_Pages)(JSP；以前的 JavaServer Pages)模板。

使用的技术和工具:

*   Java 11
*   释放弹簧
*   JSP
*   JSTL 1.2
*   Servlet API 4.0.1
*   Bootstrap 5.2.0 (webjars)
*   智能理念
*   Gradle 7.5.1
*   用于嵌入式 servlet 容器的 Gradle Gretty 插件 3 . 0 . 9(Tomcat 9 和 Jetty 9.4)
*   弹簧测试 5.2.22 .释放
*   哈姆克雷斯特 2.2
*   JUnit 5.9

目录:

*   [1。目录结构](#directory-structure)
*   [2。项目依赖性](#project-dependencies)
*   [3。项目依赖关系——树形格式](#project-dependencies-tree-format)
*   [4。弹簧控制器](#spring-controller)
*   [5。弹簧配置](#spring-configuration)
*   [6。Spring DispatcherServlet](#spring-dispatcherservlet)
*   7 .[。Spring MVC 和单元测试](#spring-mvc-and-unit-tests)
*   [8。JSP 和 JSTL](#jsp-and-jstl)
*   [9。演示](#demo)
*   10。下载源代码
*   10。参考文献

**注**
本教程不是 Spring Boot 应用，只是纯 Spring Web MVC！

## 1。目录结构

下面是这个项目的标准 Java 目录结构。

![directory structure](img/495deffe78ef48e824a92caf1d942017.png)

## 2。项目依赖性

以下是该项目的核心依赖项；`spring-webmvc`依赖是必须的，但是其他的依赖依赖于你的项目需求。

*   `spring-webmvc`–用于 Spring core 相关的 web 组件。
*   `jstl`–对于[雅加达标准标签库(JSTL；](http://web.archive.org/web/20221118095838/https://en.wikipedia.org/wiki/Jakarta_Standard_Tag_Library)原名 JavaServer Pages 标准标签库)。
*   `org.webjars.bootstrap`–用于 [WebJars](http://web.archive.org/web/20221118095838/https://www.webjars.org/) 管理客户端 web 库，例如 bootstrap。
*   `javax.servlet-api`–我们需要`servlet-api`来编译 web 应用程序，设置为`compileOnly`范围；通常，嵌入式服务器会提供这种功能。
*   `org.gretty`–使用嵌入式 servlet 容器运行这个项目，比如 Tomcat 9 或 Jetty 9.4。

**为什么是 Gretty 3 插件？**
最新的 Gretty 4 插件只支持 Tomcat 10，而 Spring MVC 5 我们需要 Tomcat 8.5 或 9，所以我们会坚持使用更老的 Gretty 3。

**为什么 Spring Web MVC 5 不运行 o Tomcat 10？**

*   Tomcat 10 在包`jakarta.*`下实现 Servlet 5 规范(Jakarta EE 9 的一部分)。
*   Spring Web MVC 5.x 在包`javax.*`下实现 Servlet 4 规范(Jakarta EE 8 的一部分)。

Spring 5 还是靠`javax.servlet.*`，最新的 Tomcat 10 靠`jakarta.servlet`。简而言之，Spring 5 无法与 Tomcat 10 兼容，因为软件包从`javax.*`重命名为`jakarta.*`。

**延伸阅读**

*   [弹簧芯 5 没有在 tomcat 10 上启动](http://web.archive.org/web/20221118095838/https://github.com/spring-projects/spring-boot/issues/25276)
*   [Spring 5 对 Servlet 4.0 API 的支持](http://web.archive.org/web/20221118095838/https://github.com/spring-projects/spring-framework/issues/17273)。

build.gradle

```java
 plugins {
	id 'org.gretty' version '3.0.9'
}
apply plugin: 'java'
apply plugin: 'war'
apply plugin: 'idea'

// JDK 11
sourceCompatibility = 11
targetCompatibility = 11

repositories {
    mavenCentral()
}

dependencies {
	compileOnly 'javax.servlet:javax.servlet-api:4.0.1'
	implementation 'javax.servlet:jstl:1.2'
	implementation 'org.springframework:spring-webmvc:5.2.22.RELEASE'
	implementation 'org.webjars:bootstrap:5.2.0'
	testImplementation 'org.junit.jupiter:junit-jupiter-engine:5.9.0'
	testImplementation 'org.springframework:spring-test:5.2.22.RELEASE'
	testImplementation 'org.hamcrest:hamcrest-core:2.2'
}

gretty {
	httpPort = 8080
	contextPath = '/'
	//servletContainer = 'jetty9.4'
	servletContainer = 'tomcat9'
}

test {
	useJUnitPlatform()
} 
```

## 3。项目依赖关系——树形格式

再次检查树结构中的项目依赖关系。

Terminal

```java
 gradle dependencies --configuration compileClasspath

> Task :dependencies

------------------------------------------------------------
Root project 'spring-mvc-hello-world-jsp'
------------------------------------------------------------

compileClasspath - Compile classpath for source set 'main'.
+--- javax.servlet:javax.servlet-api:4.0.1
+--- javax.servlet:jstl:1.2
+--- org.springframework:spring-webmvc:5.2.22.RELEASE
|    +--- org.springframework:spring-aop:5.2.22.RELEASE
|    |    +--- org.springframework:spring-beans:5.2.22.RELEASE
|    |    |    \--- org.springframework:spring-core:5.2.22.RELEASE
|    |    |         \--- org.springframework:spring-jcl:5.2.22.RELEASE
|    |    \--- org.springframework:spring-core:5.2.22.RELEASE (*)
|    +--- org.springframework:spring-beans:5.2.22.RELEASE (*)
|    +--- org.springframework:spring-context:5.2.22.RELEASE
|    |    +--- org.springframework:spring-aop:5.2.22.RELEASE (*)
|    |    +--- org.springframework:spring-beans:5.2.22.RELEASE (*)
|    |    +--- org.springframework:spring-core:5.2.22.RELEASE (*)
|    |    \--- org.springframework:spring-expression:5.2.22.RELEASE
|    |         \--- org.springframework:spring-core:5.2.22.RELEASE (*)
|    +--- org.springframework:spring-core:5.2.22.RELEASE (*)
|    +--- org.springframework:spring-expression:5.2.22.RELEASE (*)
|    \--- org.springframework:spring-web:5.2.22.RELEASE
|         +--- org.springframework:spring-beans:5.2.22.RELEASE (*)
|         \--- org.springframework:spring-core:5.2.22.RELEASE (*)
\--- org.webjars:bootstrap:5.2.0 
```

## 4。弹簧控制器

下面是一个 Spring Web MVC 控制器，用于处理对`/`和`/hello/{name}`的 Web 请求并显示消息。

HelloController.java

```java
 package com.mkyong.web;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HelloController {

    //@RequestMapping(value = "/", method = RequestMethod.GET)
    @GetMapping("/")
    public String defaultPage(ModelMap model) {
        return "hello";
    }

    //@RequestMapping(value = "/hello/{name:.+}", method = RequestMethod.GET)
    @GetMapping("/hello/{name:.+}")
    public ModelAndView hello(@PathVariable("name") String name) {

        ModelAndView model = new ModelAndView();
        model.setViewName("hello");
        model.addObject("message", name);

        return model;

    }

} 
```

## 5。弹簧配置

Spring 需要一个用于 JSP 页面的`viewResolver` bean。

SpringWebConfig.java

```java
 package com.mkyong.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import org.springframework.web.servlet.view.JstlView;

@EnableWebMvc
@Configuration
@ComponentScan({ "com.mkyong.web" })
public class SpringWebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("/webjars/");
    }

    @Bean
    public InternalResourceViewResolver viewResolver() {
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setViewClass(JstlView.class);
        viewResolver.setPrefix("/WEB-INF/views/jsp/");
        viewResolver.setSuffix(".jsp");
        return viewResolver;
    }

} 
```

## 6。Spring DispatcherServlet

这个`MyServletInitializer`将由 Servlet 容器(例如 Jetty 或 Tomcat)自动检测。

MyServletInitializer.java

```java
 package com.mkyong;

import com.mkyong.config.SpringWebConfig;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class MyServletInitializer
      extends AbstractAnnotationConfigDispatcherServletInitializer {

  // services and data sources
  @Override
  protected Class<?>[] getRootConfigClasses() {
      return new Class[0];
  }

  // controller, view resolver, handler mapping
  @Override
  protected Class<?>[] getServletConfigClasses() {
      return new Class[]{SpringWebConfig.class};
  }

  @Override
  protected String[] getServletMappings() {
      return new String[]{"/"};
  }
} 
```

## 7 .。Spring MVC 和单元测试

Spring MVC 控制器的简单单元测试。

HelloControllerTest.java

```java
 package com.mkyong;

import com.mkyong.config.SpringWebConfig;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.MvcResult;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.servlet.ModelAndView;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNull;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@ExtendWith(SpringExtension.class)
@WebAppConfiguration
@ContextConfiguration(classes = {SpringWebConfig.class})
public class HelloControllerTest {

    @Autowired
    private WebApplicationContext wac;

    private MockMvc mockMvc;

    @BeforeEach
    void setup() {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(this.wac).build();
    }

    @Test
    public void testDefaultPage() throws Exception {

        MvcResult result = this.mockMvc.perform(get("/"))
                /*.andDo(print())*/
                .andExpect(status().isOk())
                .andReturn();

        ModelAndView modelAndView = result.getModelAndView();
        assertEquals("hello", modelAndView.getViewName());
        assertNull(modelAndView.getModel().get("message"));

    }

    @Test
    public void testHelloPage() throws Exception {

        MvcResult result = this.mockMvc.perform(get("/hello/mkyong"))
                .andExpect(status().isOk())
                .andReturn();

        ModelAndView modelAndView = result.getModelAndView();
        assertEquals("hello", modelAndView.getViewName());
        assertEquals("mkyong", modelAndView.getModel().get("message"));

    }

} 
```

## 8。JSP 和 JSTL

一个显示 hello world 消息的简单 JSP + JSTL 模板也展示了如何集成 webjars 的引导和定制 CSS 和 JS 文件。

webapp/WEB-INF/views/jsp/hello.jsp

```java
 <%@ taglib prefix="spring" uri="http://www.springframework.org/tags"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html lang="en">
<head>
<title>Spring Web MVC and JSP</title>

<spring:url value="/resources/core/css/main.css" var="coreCss" />
<spring:url value="/webjars/bootstrap/5.2.0/css/bootstrap.min.css" var="bootstrapCss" />

<link href="${bootstrapCss}" rel="stylesheet" />
<link href="${coreCss}" rel="stylesheet" />
</head>

<nav class="navbar navbar-expand-md navbar-dark bg-dark fixed-top">
  <a class="navbar-brand" href="#">Mkyong.com</a>
</nav>

<main role="main" class="container">

  <div class="starter-template">
      <h1>Spring Web MVC JSP Example</h1>
      <h2>
            <c:if test="${not empty message}">
                Hello ${message}
            </c:if>

            <c:if test="${empty message}">
                Welcome!
            </c:if>
      </h2>
  </div>

</main>

<spring:url value="/resources/core/js/main.js" var="coreJs" />
<spring:url value="/webjars/bootstrap/5.2.0/js/bootstrap.min.js" var="bootstrapJs" />

<script src="${coreJs}"></script>
<script src="${bootstrapJs}"></script>

</body>
</html> 
```

## 9。演示

转到终端，项目文件夹，运行`gradle tomcatRunWar`。

Terminal

```java
 gradle tomcatRunWar

//...
INFO: Completed initialization in 628 ms
17:19:14 INFO  Tomcat 9.0.58 started and listening on port 8080
17:19:14 INFO   runs at:
17:19:14 INFO    http://localhost:8080

> Task :tomcatRunWar
Press any key to stop the server. 
```

*另外，试着用命令`gradle jettyRunWar`在嵌入式 Jetty 上运行 Spring Web MVC。*

`http://localhost:8080/`

![spring web mvc hello world JSP demo 1](img/2cb964ec7eff55edd373f3a379393d54.png)

`http://localhost:8080/hello/mkyong`

![spring web mvc hello world JSP demo 2](img/252c32e396a336a5b0db8fde7cf7c17a.png)

## 10。下载源代码

$ git 克隆[https://github.com/mkyong/spring-mvc/](http://web.archive.org/web/20221118095838/https://github.com/mkyong/spring-mvc/)

$ cd spring-mvc-hello-world-jsp

$ gradle tomcatRunWar

或者

$度喷气式滑水道

请访问 http://localhost:8080/

请访问 http://localhost:8080/hello/mkyong

## 10。参考文献

*   [Spring MVC hello world 示例(Maven 和百里香叶)](http://web.archive.org/web/20221118095838/https://mkyong.com/spring-mvc/spring-mvc-hello-world-example/)
*   [Apache Tomcat 版本](http://web.archive.org/web/20221118095838/https://tomcat.apache.org/whichversion.html)
*   [格雷尔·格雷蒂插件](http://web.archive.org/web/20221118095838/https://plugins.gradle.org/plugin/org.gretty)
*   格雷蒂·吉图布
*   [格雷蒂文档](http://web.archive.org/web/20221118095838/https://gretty-gradle-plugin.github.io/gretty-doc/)
*   [构建 Java 应用程序示例](http://web.archive.org/web/20221118095838/https://docs.gradle.org/current/samples/sample_building_java_applications.html)
*   [维基百科–模型–视图–控制器](http://web.archive.org/web/20221118095838/https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)
*   [维基百科–雅加达服务器页面](http://web.archive.org/web/20221118095838/https://en.wikipedia.org/wiki/Jakarta_Server_Pages)
*   [Spring Boot Hello World 示例–百里香叶](http://web.archive.org/web/20221118095838/https://mkyong.com/spring-boot/spring-boot-hello-world-example-thymeleaf/)
*   [Spring 5，Embedded Tomcat 8，和 Gradle:快速教程](http://web.archive.org/web/20221118095838/https://auth0.com/blog/spring-5-embedded-tomcat-8-gradle-tutorial/)
*   [弹簧启动不适用于 Tomcat 10](http://web.archive.org/web/20221118095838/https://github.com/spring-projects/spring-boot/issues/22414)

<input type="hidden" id="mkyong-current-postId" value="13452">