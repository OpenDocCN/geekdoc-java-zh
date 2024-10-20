# 在 Spring MVC 中配置多视图解析器优先级

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/configure-multiple-view-resolvers-priority-in-spring-mvc/>

## 问题

在 Spring MVC 应用程序中，你经常会应用一些视图解析器策略来解析视图名。例如，将三个视图解析器组合在一起:**InternalResourceViewResolver**、**ResourceBundleViewResolver**和 **XmlViewResolver** 。

```java
 <beans ...>
	<bean class="org.springframework.web.servlet.view.XmlViewResolver">
	      <property name="location">
	         <value>/WEB-INF/spring-views.xml</value>
	      </property>
	</bean>

	<bean class="org.springframework.web.servlet.view.ResourceBundleViewResolver">
	      <property name="basename" value="spring-views" />
	</bean>

	<bean id="viewResolver"
	      class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
              <property name="prefix">
                 <value>/WEB-INF/pages/</value>
              </property>
              <property name="suffix">
                 <value>.jsp</value>
              </property>
        </bean>
</beans> 
```

但是，如果返回一个视图名，将使用哪个视图解析器策略呢？

## 解决办法

如果应用了多视图解析器策略，您必须通过“ **order** 属性声明优先级，其中**较低的顺序值具有较高的优先级**，例如:

```java
 <beans ...>
	<bean class="org.springframework.web.servlet.view.XmlViewResolver">
	     <property name="location">
	        <value>/WEB-INF/spring-views.xml</value>
	     </property>
	     <property name="order" value="0" />
	</bean>

	<bean class="org.springframework.web.servlet.view.ResourceBundleViewResolver">
	     <property name="basename" value="spring-views" />
	     <property name="order" value="1" />
	</bean>

	<bean id="viewResolver"
	      class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
              <property name="prefix">
                 <value>/WEB-INF/pages/</value>
              </property>
              <property name="suffix">
                 <value>.jsp</value>
              </property>
	      <property name="order" value="2" />
        </bean>
</beans> 
```

现在，如果返回视图名称，视图解析策略按以下顺序工作:

```java
 XmlViewResolver --> ResourceBundleViewResolver --> InternalResourceViewResolver 
```

**Note**
The **InternalResourceViewResolver** must always **assign with the lowest priority** (largest order number), because it will resolve the view no matter what view name is returned. It caused other view resolvers have no chance to resolve the view if they have lower priority.

## 下载源代码

Download it – [SpringMVC-ViewResolver-Priority-Example.zip](http://web.archive.org/web/20200616172845/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVC-ViewResolver-Priority-Example.zip) (7KB)

## 参考

1.  [Spring MVC InternalResourceViewResolver 示例](http://web.archive.org/web/20200616172845/http://www.mkyong.com/spring-mvc/spring-mvc-internalresourceviewresolver-example/)
2.  [Spring MVC XmlViewResolver 示例](http://web.archive.org/web/20200616172845/http://www.mkyong.com/spring-mvc/spring-mvc-xmlviewresolver-example/)
3.  [Spring MVC ResourceBundleViewResolver 示例](http://web.archive.org/web/20200616172845/http://www.mkyong.com/spring-mvc/spring-mvc-resourcebundleviewresolver-example/)

Tags : [spring mvc](http://web.archive.org/web/20200616172845/https://mkyong.com/tag/spring-mvc/)<input type="hidden" id="mkyong-current-postId" value="6703">