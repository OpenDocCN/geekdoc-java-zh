# Spring AOP 错误:无法代理目标类，因为 CGLIB2 不可用

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-aop-error-cannot-proxy-target-class-because-cglib2-is-not-available/>

在 Spring AOP 中，您必须将 **cglib** 库包含到您的构建路径中，以避免“**无法代理目标类，因为 CGLIB2 不可用**”错误消息。

```java
 Exception in thread "main" org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'customerServiceProxy': FactoryBean threw exception on object creation; nested exception is org.springframework.aop.framework.AopConfigException: Cannot proxy target class because CGLIB2 is not available. Add CGLIB to the class path or specify proxy interfaces.
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport$1.run(FactoryBeanRegistrySupport.java:127)
	at java.security.AccessController.doPrivileged(Native Method)
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.doGetObjectFromFactoryBean(FactoryBeanRegistrySupport.java:116)
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.getObjectFromFactoryBean(FactoryBeanRegistrySupport.java:91)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getObjectForBeanInstance(AbstractBeanFactory.java:1288)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:217)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:185)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:164)
	at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:880)
	at com.mkyong.common.App.main(App.java:14)
Caused by: org.springframework.aop.framework.AopConfigException: Cannot proxy target class because CGLIB2 is not available. Add CGLIB to the class path or specify proxy interfaces.
	at org.springframework.aop.framework.DefaultAopProxyFactory.createAopProxy(DefaultAopProxyFactory.java:67)
	at org.springframework.aop.framework.ProxyCreatorSupport.createAopProxy(ProxyCreatorSupport.java:106)
	at org.springframework.aop.framework.ProxyFactoryBean.getSingletonInstance(ProxyFactoryBean.java:317)
	at org.springframework.aop.framework.ProxyFactoryBean.getObject(ProxyFactoryBean.java:243)
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport$1.run(FactoryBeanRegistrySupport.java:121)
	... 9 more 
```

## 解决办法

您可以从…下载 cglib 库

1.Cglib 官方网站
[http://cglib.sourceforge.net/](http://web.archive.org/web/20190216043613/http://cglib.sourceforge.net/)

2.美文库
[http://repo1.maven.org/maven2/cglib/cglib/2.2/](http://web.archive.org/web/20190216043613/http://repo1.maven.org/maven2/cglib/cglib/2.2/)

如果您正在使用 Maven，您可以只包含 Maven 依赖项。

```java
 <!-- AOP dependency -->
    <dependency>
    	<groupId>cglib</groupId>
	<artifactId>cglib</artifactId>
	<version>2.2</version>
    </dependency> 
```

[aop](http://web.archive.org/web/20190216043613/http://www.mkyong.com/tag/aop/) [spring](http://web.archive.org/web/20190216043613/http://www.mkyong.com/tag/spring/)![](img/2c372dd809f1b92616506c49b9db00be.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190216043613/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3990">







