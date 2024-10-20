# GAE + JSF:无法实例化 expression factory“com . sun . El . expression factory impl”

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/gae-jsf-unable-to-instantiate-expressionfactory-com-sun-el-expressionfactoryimpl/>

## 问题

JSF 应用程序能够部署在本地 GAE 环境中，开发环境如下:

1.  JSF 2.1.7
2.  谷歌应用引擎 SDK 1.6.3

但是在 GAE 生产环境中部署时出现错误消息。下面是 GAE 记录的错误消息。

```java
 com.sun.faces.config.ConfigureListener installExpressionFactory: Unable to instantiate ExpressionFactory 'com.sun.el.ExpressionFactoryImpl'
E 2012-04-24 03:37:37.989
com.sun.faces.config.ConfigureListener contextInitialized: Critical error during deployment: 
com.sun.faces.config.ConfigurationException: 
	It appears the JSP version of the container is older than 2.1 and unable to locate the EL RI expression factory, 
	com.sun.el.ExpressionFactoryImpl.  If not using JSP or the EL RI, make sure the context initialization parameter, 
	com.sun.faces.expressionFactory, is properly set.

	at com.sun.faces.config.ConfigureListener.registerELResolverAndListenerWithJsp(ConfigureListener.java:662)
	at com.sun.faces.config.ConfigureListener.contextInitialized(ConfigureListener.java:243)
	at org.mortbay.jetty.handler.ContextHandler.startContext(ContextHandler.java:548)
	at org.mortbay.jetty.servlet.Context.startContext(Context.java:136)
	at org.mortbay.jetty.webapp.WebAppContext.startContext(WebAppContext.java:1250)
	at org.mortbay.jetty.handler.ContextHandler.doStart(ContextHandler.java:517)
	at org.mortbay.jetty.webapp.WebAppContext.doStart(WebAppContext.java:467)
	at org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:50)
	at com.google.apphosting.runtime.jetty.AppVersionHandlerMap.createHandler(AppVersionHandlerMap.java:202)
	at com.google.apphosting.runtime.jetty.AppVersionHandlerMap.getHandler(AppVersionHandlerMap.java:171)
	at com.google.apphosting.runtime.jetty.JettyServletEngineAdapter.serviceRequest(JettyServletEngineAdapter.java:123)
	at com.google.apphosting.runtime.JavaRuntime$RequestRunnable.run(JavaRuntime.java:446)
	at com.google.tracing.TraceContext$TraceContextRunnable.runInContext(TraceContext.java:449)
	at com.google.tracing.TraceContext$TraceContextRunnable$1.run(TraceContext.java:455)
	at com.google.tracing.TraceContext.runInContext(TraceContext.java:695)
	at com.google.tracing.TraceContext$AbstractTraceContextCallback.runInInheritedContextNoUnref(TraceContext.java:333)
	at com.google.tracing.TraceContext$AbstractTraceContextCallback.runInInheritedContext(TraceContext.java:325)
	at com.google.tracing.TraceContext$TraceContextRunnable.run(TraceContext.java:453)
	at com.google.apphosting.runtime.ThreadGroupPool$PoolEntry.run(ThreadGroupPool.java:251)
	at java.lang.Thread.run(Thread.java:679) 
```

 ## 解决办法

GAE + JSF 需要 **el-ri.jar** 。要解决它，找到并获取 **el-ri.jar** ，并将其复制到您的“ **/war/lib** ”文件夹中。

**Where to get el-ri.jar?**
The faster way is get the **el-ri.jar** from [Maven repository](http://web.archive.org/web/20190302180431/http://search.maven.org/remotecontent?filepath=com/sun/el/el-ri/1.0/el-ri-1.0.jar). ## 参考

1.  [搜索 Maven 知识库](http://web.archive.org/web/20190302180431/http://search.maven.org/)
2.  [带有 JSF2、CDI 的谷歌应用引擎](http://web.archive.org/web/20190302180431/http://javamomentum.be/2011/04/google-app-engine-with-jsf2-cdi/)

[gae](http://web.archive.org/web/20190302180431/http://www.mkyong.com/tag/gae/) [jsf2](http://web.archive.org/web/20190302180431/http://www.mkyong.com/tag/jsf2/)







