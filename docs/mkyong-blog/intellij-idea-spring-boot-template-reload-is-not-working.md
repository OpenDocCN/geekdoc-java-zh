# intellij IDEA–弹簧引导重新加载静态文件不起作用

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/intellij-idea-spring-boot-template-reload-is-not-working/>

在 Eclipse 中，只要包含 [Spring Boot 开发工具](http://web.archive.org/web/20220827143757/https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html)依赖项，热插拔和静态文件重载就会神奇地启用。对于 Intellij IDE，我们需要额外的步骤来启用它。

## 1.Spring Boot 开发工具

启用 Spring Boot 开发工具后:

*   对视图或资源的任何更改都可以直接在浏览器中看到，无需重启，只需刷新浏览器。
*   对代码的任何修改都会自动重启 Spring 容器。

首先，包括`Spring Boot Dev Tools`依赖项:

pom.xml

```java
 <!-- hot swapping, enable live reload -->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-devtools</artifactId>
      <optional>true</optional>
  </dependency> 
```

## 2.自动构建项目

文件–>设置–>构建、执行、部署–>编译器–>检查此`Build project automatically`

![](img/26823bf63b2a74def760ea2781d9b73a.png)

## 3.Intellij 注册表

3.1 按`SHIFT+CTRL+A` (Win/*nix)或`Command+SHIFT+A` (Mac)打开一个弹出窗口，键入`registry`

![](img/abb47327209db9924753999a14bbf111.png)

3.2 找到并启用此选项`compiler.automake.allow.when.app.running`

![](img/68abdb306bebd10897da4be440e71724.png)

完成了。现在，热插拔和静态文件自动重新加载应该启用。

**In Menu -> Build -> Build Project (CTRL + F9)**
If the static files are not reloaded, press `CTRL+F9` to force a reload.

## 参考

*   [Spring Boot 开发工具](http://web.archive.org/web/20220827143757/https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html)
*   [Spring Boot Hello World 示例–百里香叶](http://web.archive.org/web/20220827143757/https://www.mkyong.com/spring-boot/spring-boot-hello-world-example-thymeleaf/)

<input type="hidden" id="mkyong-current-postId" value="14247">