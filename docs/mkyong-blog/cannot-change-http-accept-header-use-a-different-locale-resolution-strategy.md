# 无法更改 HTTP accept 标头-使用不同的区域设置解析策略

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/cannot-change-http-accept-header-use-a-different-locale-resolution-strategy/>

## 问题

在 Spring MVC 应用程序中，当用"**org . Spring framework . web . servlet . i18n . localechangeinterceptor**"更改区域设置时，遇到以下错误

```java
 java.lang.UnsupportedOperationException: 
     Cannot change HTTP accept header - use a different locale resolution strategy
     ...AcceptHeaderLocaleResolver.setLocale(AcceptHeaderLocaleResolver.java:45) 
```

 ## 解决办法

在 Spring MVC 应用程序中，如果不配置 Spring 的 LocaleResolver，它将使用默认的**AcceptHeaderLocaleResolver**，这不允许更改区域设置。要解决这个问题，请尝试在 Spring bean 配置文件中声明一个**session locale resolver**bean，它应该适合大多数情况。

```java
 <beans ...

	<bean id="localeResolver"
		class="org.springframework.web.servlet.i18n.SessionLocaleResolver">
		<property name="defaultLocale" value="en" />
	</bean>

	<bean id="localeChangeInterceptor"
		class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor">
		<property name="paramName" value="language" />
	</bean>

	<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping" >
		<property name="interceptors">
			<list>
				<ref bean="localeChangeInterceptor" />
			</list>
		</property>
	</bean>

</beans> 
```

 ## 参考

1.  [locale solver 文档](http://web.archive.org/web/20190225100934/http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/servlet/class-use/LocaleResolver.html)

[spring mvc](http://web.archive.org/web/20190225100934/http://www.mkyong.com/tag/spring-mvc/)







