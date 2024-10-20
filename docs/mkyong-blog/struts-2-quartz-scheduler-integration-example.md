# Struts 2 + Quartz 2 调度器集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-quartz-scheduler-integration-example/>

**Updated 2012-07-24**
Article is updated to use latest Struts 2 and Quartz 2, and tested on Tomcat 6 and 7.

Struts 2 没有附带任何现成的"`Struts2-Quartz.jar`"插件，黑客使用一个标准的 Servlet 监听器将两个框架链接在一起。看到关系了吗

```java
 Struts 2 <-- (Listener)--> Quartz <---> Scheduler task 
```

在本教程中，我们将向您展示如何将 Struts 2 和 Quartz scheduler 框架集成在一起。

使用的工具:

1.  支柱 2.3.4
2.  石英 2.1.5
3.  Tomcat 6 或 7
4.  maven3
5.  Eclipse 4.2

## 1.依赖库

没有太多的依赖，你只需要 Struts 2 和 Quartz jar 文件。

*文件:pom.xml*

```java
 ...
   <dependencies>

	<!-- Struts 2 -->
	<dependency>
		<groupId>org.apache.struts</groupId>
		<artifactId>struts2-core</artifactId>
		<version>2.3.4</version>
	</dependency>

	<!-- Quartz framework -->
	<dependency>
		<groupId>org.quartz-scheduler</groupId>
		<artifactId>quartz</artifactId>
		<version>2.1.5</version>
	</dependency>

        <!-- for javax.servlet.* classes -->
	<dependency>
		<groupId>org.apache.tomcat</groupId>
		<artifactId>servlet-api</artifactId>
		<version>6.0.35</version>
	</dependency>

  </dependencies> 
  ... 
```

 ## 2.调度程序作业

创建一个 Quartz 作业并打印出一行。

*文件:SchedulerJob.java*

```java
 package com.mkyong.quartz;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class SchedulerJob implements Job {
	public void execute(JobExecutionContext context)
		throws JobExecutionException {

		System.out.println("Struts 2.3.4 + Quartz 2.1.5");

	}
} 
```

 ## 3.Servlet 监听器

创建标准的 servlet 监听器类，以完成集成工作。它在`contextInitialized()`方法中调用 Quartz 调度程序框架。在 Servlet 容器初始化期间，这个`contextInitialized()`方法将被自动执行。

*文件:QuartzSchedulerListener.java*

```java
 package com.mkyong.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import org.quartz.CronScheduleBuilder;
import org.quartz.JobBuilder;
import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.SchedulerException;
import org.quartz.Trigger;
import org.quartz.TriggerBuilder;
import org.quartz.impl.StdSchedulerFactory;
import com.mkyong.quartz.SchedulerJob;

public class QuartzSchedulerListener implements ServletContextListener {

	public void contextDestroyed(ServletContextEvent arg0) {
		//
	}

	public void contextInitialized(ServletContextEvent arg0) {

		JobDetail job = JobBuilder.newJob(SchedulerJob.class)
			.withIdentity("anyJobName", "group1").build();

		try {

			Trigger trigger = TriggerBuilder
			  .newTrigger()
			  .withIdentity("anyTriggerName", "group1")
			  .withSchedule(
			     CronScheduleBuilder.cronSchedule("0/10 * * * * ?"))
			  .build();

			Scheduler scheduler = new StdSchedulerFactory().getScheduler();
			scheduler.start();
			scheduler.scheduleJob(job, trigger);

		} catch (SchedulerException e) {
			e.printStackTrace();
		}

	}
} 
```

## 4.web.xml

将监听器类`QuartzSchedulerListener.java`放到`web.xml`文件中。

*文件:web.xml*

```java
 <!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
	<display-name>Struts 2 Web Application</display-name>

	<filter>
	  <filter-name>struts2</filter-name>
	  <filter-class>
            org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
          </filter-class>
	</filter>

	<filter-mapping>
	  <filter-name>struts2</filter-name>
	  <url-pattern>/*</url-pattern>
	</filter-mapping>

	<listener>
	  <listener-class>
            com.mkyong.listener.QuartzSchedulerListener
          </listener-class>
	</listener>

</web-app> 
```

## 5.演示

Struts 2 项目启动后，注册的监听器类`QuartzSchedulerListener.java`将被触发，每隔 10 秒调用 Quartz 调度作业执行`SchedulerTask.printSchedulerMessage()`方法。

```java
 INFO: Overriding property struts.i18n.reload - old value: false new value: true
Jul 23, 2012 4:56:47 PM com.opensymphony.xwork2.util.logging.jdk.JdkLogger info
INFO: Overriding property struts.configuration.xml.reload - old value: false new value: true
Jul 23, 2012 4:56:48 PM org.apache.coyote.http11.Http11Protocol start
INFO: Starting Coyote HTTP/1.1 on http-8080
Jul 23, 2012 4:56:48 PM org.apache.jk.common.ChannelSocket init
INFO: JK: ajp13 listening on /0.0.0.0:8009
Jul 23, 2012 4:56:48 PM org.apache.jk.server.JkMain start
INFO: Jk running ID=0 time=0/38  config=null
Jul 23, 2012 4:56:48 PM org.apache.catalina.startup.Catalina start
INFO: Server startup in 1215 ms
Struts 2.3.4 + Quartz 2.1.5
Struts 2.3.4 + Quartz 2.1.5 
```

## 下载源代码

Download it –[Struts2-Quartz-Example.zip](http://web.archive.org/web/20190223082206/http://www.mkyong.com/wp-content/uploads/2010/07/Struts2-Quartz-Example.zip) (22 KB)

## 参考

1.  [石英调度器示例](http://web.archive.org/web/20190223082206/http://www.mkyong.com/java/quartz-scheduler-example/)
2.  [Struts + Quartz 集成示例](http://web.archive.org/web/20190223082206/http://www.mkyong.com/struts/struts-quartz-scheduler-integration-example/)
3.  [支柱 2 +弹簧+石英集成示例](http://web.archive.org/web/20190223082206/http://www.mkyong.com/struts2/struts-2-spring-quartz-scheduler-integration-example/)
4.  [支柱+弹簧+石英集成示例](http://web.archive.org/web/20190223082206/http://www.mkyong.com/struts/struts-spring-quartz-scheduler-integration-example/)

[integration](http://web.archive.org/web/20190223082206/http://www.mkyong.com/tag/integration/) [quartz](http://web.archive.org/web/20190223082206/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190223082206/http://www.mkyong.com/tag/scheduler/) [struts2](http://web.archive.org/web/20190223082206/http://www.mkyong.com/tag/struts2/)







