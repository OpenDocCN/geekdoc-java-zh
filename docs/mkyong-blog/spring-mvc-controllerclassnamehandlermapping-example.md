# spring MVC controllerclassnamehandler 映射示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-controllerclassnamehandlermapping-example/>

在 Spring MVC 中，**ControllerClassNameHandlerMapping**使用约定将请求的 URL 映射到控制器(约定优先于配置)。它接受类名，删除“Controller”后缀(如果存在的话),并返回剩余的文本，小写并以“/”开头。

参见下面几个例子来演示这个`ControllerClassNameHandlerMapping`类的用法。

## 1.之前和之后

默认情况下，Spring MVC 使用的是`BeanNameUrlHandlerMapping`处理程序映射。

```java
 <beans ...>

  <bean name="/welcome.htm" 
        class="com.mkyong.common.controller.WelcomeController" />

  <bean name="/helloGuest.htm" 
        class="com.mkyong.common.controller.HelloGuestController" />

</beans> 
```

为了启用**ControllerClassNameHandlerMapping**，在 bean 配置文件中声明了它，现在**不再需要控制器的 bean 的名称**。

```java
 <beans ...>

  <bean 
   class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" />

  <bean class="com.mkyong.common.controller.WelcomeController" />

  <bean class="com.mkyong.common.controller.HelloGuestController" />

</beans> 
```

现在，Spring MVC 通过以下约定映射请求的 URL:

```java
 WelcomeController -> /welcome*
HelloGuestController -> /helloguest* 
```

1.  /welcome . htm –> welcome controller。
2.  /welcome home . htm –> welcome controller。
3.  /hello guest . htm –> hello guest controller。
4.  /hello guest 12345 . htm –> hello guest controller。
5.  /helloGuest.htm，无法映射**/hello guest ***,“g”大小写不匹配。

 ## 2.区分大小写

为了解决上述区分大小写的问题，声明了“ **caseSensitive** ”属性并将其设置为 true。

```java
 <beans ...>

  <bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" >
       <property name="caseSensitive" value="true" />
  </bean>

  <bean class="com.mkyong.common.controller.WelcomeController" />

  <bean class="com.mkyong.common.controller.HelloGuestController" />

</beans> 
```

现在，Spring MVC 通过以下约定映射请求的 URL:

```java
 WelcomeController -> /welcome*
HelloGuestController -> /helloGuest* 
```

1.  /hello guest . htm –> hello guest controller。
2.  /helloguest.htm，未能映射“/hello guest *”,“G”大小写不匹配。

 ## 3\. pathPrefix

此外，您可以指定一个前缀来映射请求的 URL，声明一个“ **pathPrefix** 属性。

```java
 <beans ...>

  <bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" >
	 <property name="caseSensitive" value="true" />
	 <property name="pathPrefix" value="/customer" />
  </bean>

  <bean class="com.mkyong.common.controller.WelcomeController" />

  <bean class="com.mkyong.common.controller.HelloGuestController" />

</beans> 
```

现在，Spring MVC 通过以下约定映射请求的 URL:

```java
 WelcomeController -> /customer/welcome*
HelloGuestController -> /customer/helloGuest* 
```

1.  /customer/welcome . htm –> welcome controller。
2.  /customer/hello guest . htm –> hello guest controller。
3.  /welcome.htm，失败。
4.  /helloGuest.htm，失败。

## 下载源代码

Download it – [SpringMVC-ControllerClassNameHandlerMapping-Example.zip](http://web.archive.org/web/20190210101626/http://www.mkyong.com/wp-content/uploads/2010/07/SpringMVC-ControllerClassNameHandlerMapping-Example.zip) (7KB)

## 参考

1.  [ControllerClassNameHandlerMapping javadoc](http://web.archive.org/web/20190210101626/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/mvc/support/ControllerClassNameHandlerMapping.html)

[spring mvc](http://web.archive.org/web/20190210101626/http://www.mkyong.com/tag/spring-mvc/)







