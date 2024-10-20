# quartz:org . quartz . scheduler config 异常:线程计数必须大于 0

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/quartz-org-quartz-schedulerconfigexception-thread-count-must-be-0/>

使用 Quartz 2，当运行项目时，遇到以下错误信息？

```java
 org.quartz.SchedulerConfigException: Thread count must be > 0
	at org.quartz.simpl.SimpleThreadPool.initialize(SimpleThreadPool.java:245)
	at org.quartz.impl.StdSchedulerFactory.instantiate(StdSchedulerFactory.java:1255)
	at org.quartz.impl.StdSchedulerFactory.getScheduler(StdSchedulerFactory.java:1484)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4791)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5285)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1559)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1549)
	at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:303)
	at java.util.concurrent.FutureTask.run(FutureTask.java:138)
	at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:886)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:908)
	at java.lang.Thread.run(Thread.java:680) 
```

## 解决办法

您已经定义了一个“quartz.properties”文件，并覆盖了默认 quartz 的线程计数值。

要修复它，您可以:

1.  删除你的“quartz.properties”，通常这个文件只在高级配置时需要。通常，简单的项目不需要这个。
2.  正确定义了一个`org.quartz.threadPool.threadCount`值。

*文件:quartz . properties*–工作样品。

```java
 org.quartz.scheduler.instanceName = MyScheduler
org.quartz.threadPool.threadCount = 3
org.quartz.jobStore.class = org.quartz.simpl.RAMJobStore
org.quartz.plugin.jobInitializer.class =org.quartz.plugins.xml.XMLSchedulingDataProcessorPlugin 
org.quartz.plugin.jobInitializer.fileNames = quartz-config.xml 
org.quartz.plugin.jobInitializer.failOnFileNotFound = true 
```

 ## 参考

1.  [石英配置参考](http://web.archive.org/web/20190222093208/http://www.quartz-scheduler.org/documentation/quartz-2.1.x/configuration)
2.  [JSF 2 +石英示例](http://web.archive.org/web/20190222093208/http://www.mkyong.com/jsf2/jsf-2-quartz-2-example/)

[quartz](http://web.archive.org/web/20190222093208/http://www.mkyong.com/tag/quartz/)![](img/6f7187d4ca228957ac50ac0439be2904.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190222093208/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="11280">







