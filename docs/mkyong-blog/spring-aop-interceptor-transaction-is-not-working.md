# Spring AOP 拦截器事务不工作

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-aop-interceptor-transaction-is-not-working/>

## 问题

Spring AOP 事务在下列拦截器中不起作用？

```java
 <bean id="testAutoProxyCreator"
    class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">
	<property name="interceptorNames">
		<list>
			<idref bean="urlInterceptorInsert" />
			<idref bean="urlInterceptorCommit" />
			<idref bean="urlInterceptorRelease" />
			<idref bean="matchGenericTxInterceptor" />
		</list>
	</property>
	<property name="beanNames">
		<list>
			<idref local="urlBo" />
		</list>
	</property>
</bean> 
```

“**matchgenericxinterceptor**”事务拦截器，本应该拦截`urlInterceptorInsert`、`urlInterceptorCommit`、`urlInterceptorRelease`，但是没有按预期工作？

## 解决办法

这 3 个拦截器在事务管理器拦截器(**matchgenericxinterceptor**)之前执行。

要解决这个问题，您必须改变拦截器 xml 文件的顺序，如下所示(将**matchGenericTxInterceptor**放在顶部)。

```java
 <bean id="testAutoProxyCreator"
        class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">
	<property name="interceptorNames">
		<list>
            <idref bean="matchGenericTxInterceptor" />
			<idref bean="urlInterceptorInsert" />
			<idref bean="urlInterceptorCommit" />
			<idref bean="urlInterceptorRelease" />
		</list>
	</property>
	<property name="beanNames">
		<list>
			<idref local="urlBo" />
		</list>
	</property>
</bean> 
```

**Note**
The sequence of Spring AOP interceptors do affect the functionality.Tags : [aop](http://web.archive.org/web/20210818172544/https://mkyong.com/tag/aop/) [spring](http://web.archive.org/web/20210818172544/https://mkyong.com/tag/spring/)<input type="hidden" id="mkyong-current-postId" value="170">