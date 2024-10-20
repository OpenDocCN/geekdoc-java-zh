# spring MVC SimpleUrlHandlerMapping 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-simpleurlhandlermapping-example/>

在 Spring MVC 应用中， **SimpleUrlHandlerMapping** 是最灵活的处理程序映射类，它允许开发者显式地指定 URL 模式和处理程序的映射。

简单 URL handler 映射可以用两种方式声明。

## 1.方法 1–道具钥匙

属性键是 URL 模式，而属性值是处理程序 id 或名称。

```java
 <beans ...>

	<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
	   <property name="mappings">
		<props>
		   <prop key="/welcome.htm">welcomeController</prop>
		   <prop key="/*/welcome.htm">welcomeController</prop>
		   <prop key="/helloGuest.htm">helloGuestController</prop>
		 </props>
	   </property>
	</bean>

	<bean id="welcomeController" 
		class="com.mkyong.common.controller.WelcomeController" />

	<bean id="helloGuestController" 
		class="com.mkyong.common.controller.HelloGuestController" />

</beans> 
```

 ## 2.方法 1–价值

左侧是 URL 模式，右侧是处理程序 id 或名称，用等号“=”隔开。

```java
 <beans ...>

	<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
	   <property name="mappings">
		<value>
		   /welcome.htm=welcomeController
		   /*/welcome.htm=welcomeController
		   /helloGuest.htm=helloGuestController
		</value>
	   </property>
	</bean>

	<bean id="welcomeController" 
		class="com.mkyong.common.controller.WelcomeController" />

	<bean id="helloGuestController" 
		class="com.mkyong.common.controller.HelloGuestController" />

</beans> 
```

 ## 3.演示

两者都定义了相同的处理程序映射。

1.  /welcome . htm –> welcome controller。
2.  /{ any }/welcome . htm –> welcome controller。
3.  /hello guest . htm –> hello guest controller。

## 下载源代码

Download it – [SpringMVC-SimpleUrlHandlerMapping-Example.zip](http://web.archive.org/web/20190226181023/http://www.mkyong.com/wp-content/uploads/2010/07/SpringMVC-SimpleUrlHandlerMapping-Example.zip) (7KB)

## 参考

1.  [SimpleUrlHandlerMapping javadoc](http://web.archive.org/web/20190226181023/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/handler/SimpleUrlHandlerMapping.html)

[spring mvc](http://web.archive.org/web/20190226181023/http://www.mkyong.com/tag/spring-mvc/) [url mapping](http://web.archive.org/web/20190226181023/http://www.mkyong.com/tag/url-mapping/)







