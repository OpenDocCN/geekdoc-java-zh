# JSF 2 +石英 2 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jsf2/jsf-2-quartz-2-example/>

在本教程中，我们将向您展示如何通过 Quartz 库中的`QuartzInitializerListener`监听器类在 JSF web 应用程序中运行 Quartz 作业。这个解决方案不仅适用于 JSF 2，这个概念也适用于几乎所有的标准 Java web 应用程序。

使用的工具:

1.  JSF 2.1.11
2.  石英
3.  maven3
4.  Eclipse 4.2
5.  Tomcat 7

之前的 [JSF 2.0 hello world 示例](http://web.archive.org/web/20190224144312/http://www.mkyong.com/jsf2/jsf-2-0-hello-world-example/)被重用，我们将通过`QuartzInitializerListener`监听器类增强它以支持 Quartz 作业。

本教程仅关注石英集成，对于 JSF，请阅读以上 JSF hello world 示例。

## 1.项目文件夹

检查最终的项目目录结构。

![project directory structure](img/5dde32659ec75ff27625483d2f2b91f2.png "jsf-quartz-project-directory") ## 2.属国

要在 Tomcat 上部署，您需要许多 JSF 依赖项。有关详细信息，请阅读 XML 注释。

*文件:pom.xml*

```java
 <dependencies>
...
                <!-- JSF 2 libraries -->
		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-api</artifactId>
			<version>2.1.11</version>
		</dependency>
		<dependency>
			<groupId>com.sun.faces</groupId>
			<artifactId>jsf-impl</artifactId>
			<version>2.1.11</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
		</dependency>

                <!-- Tomcat 6 need this -->
		<dependency>
			<groupId>com.sun.el</groupId>
			<artifactId>el-ri</artifactId>
			<version>1.0</version>
		</dependency>

		<!-- Quartz scheduler framework -->
		<dependency>
			<groupId>org.quartz-scheduler</groupId>
			<artifactId>quartz</artifactId>
			<version>2.1.5</version>
		</dependency>

		<!-- Quartz need transaction -->
		<dependency>
			<groupId>javax.transaction</groupId>
			<artifactId>jta</artifactId>
			<version>1.1</version>
		</dependency>
... 
```

 ## 3.石英工作

创建一个 Quartz 作业类。该课程将在以后安排和运行。

*文件:SchedulerJob.java*

```java
 package com.mkyong.scheduler;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class SchedulerJob implements Job {

	@Override
	public void execute(JobExecutionContext context)
		throws JobExecutionException {

		System.out.println("JSF 2 + Quartz 2 example");

	}

} 
```

## 4.石英结构

创建`quartz.properties`和`quartz-config.xml`，将其放在 resources“文件夹”(Maven 结构)中，对于非 Maven 项目，确保它可以位于项目类路径中。

*文件:quartz . properties*–配置 Quartz 实例并从`quartz-config.xml`读取设置

```java
 org.quartz.scheduler.instanceName = MyScheduler
org.quartz.threadPool.threadCount = 3
org.quartz.jobStore.class = org.quartz.simpl.RAMJobStore
org.quartz.plugin.jobInitializer.class =org.quartz.plugins.xml.XMLSchedulingDataProcessorPlugin 
org.quartz.plugin.jobInitializer.fileNames = quartz-config.xml 
org.quartz.plugin.jobInitializer.failOnFileNotFound = true 
```

*文件:quartz-config . XML*–配置触发器运行`com.mkyong.scheduler.SchedulerJob`

```java
 <?xml version="1.0" encoding="UTF-8"?>

<job-scheduling-data

	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.quartz-scheduler.org/xml/JobSchedulingData 
	http://www.quartz-scheduler.org/xml/job_scheduling_data_1_8.xsd"
	version="1.8">

	<schedule>
		<job>
			<name>AJob</name>
			<group>AGroup</group>
			<description>Print a welcome message</description>
			<job-class>com.mkyong.scheduler.SchedulerJob</job-class>
		</job>

		<trigger>
			<cron>
				<name>dummyTriggerName</name>
				<job-name>AJob</job-name>
				<job-group>AGroup</job-group>
				<!-- It will run every 5 seconds -->
				<cron-expression>0/5 * * * * ?</cron-expression>
			</cron>
		</trigger>
	</schedule>
</job-scheduling-data> 
```

**Note**
For detail explanation, please read this [Quartz configuration reference](http://web.archive.org/web/20190224144312/http://www.quartz-scheduler.org/documentation/quartz-2.1.x/configuration) article.

## 5.集成石英

这是整合发生的地方。在`web.xml`文件中将`org.quartz.ee.servlet.QuartzInitializerListener`声明为监听器类。

*文件:web.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<web-app ...>

	<listener>
		<listener-class>
			org.quartz.ee.servlet.QuartzInitializerListener
		</listener-class>
	</listener>

</web-app> 
```

## 6.演示

在项目启动期间，Quartz 启动并每隔 5 秒运行一次计划的作业。

```java
 Jul 26, 2012 3:32:18 PM org.apache.coyote.AbstractProtocol start
INFO: Starting ProtocolHandler ["http-bio-8080"]
Jul 26, 2012 3:32:18 PM org.apache.coyote.AbstractProtocol start
INFO: Starting ProtocolHandler ["ajp-bio-8009"]
Jul 26, 2012 3:32:18 PM org.apache.catalina.startup.Catalina start
INFO: Server startup in 3591 ms
JSF 2 + Quartz 2 example
JSF 2 + Quartz 2 example
JSF 2 + Quartz 2 example 
```

## 下载源代码

Download it – [JSF-Quartz-Example.zip](http://web.archive.org/web/20190224144312/http://www.mkyong.com/wp-content/uploads/2012/07/JSF-Quartz-Example.zip) (25 kb)

## 参考

1.  [石英 2 调度器教程](http://web.archive.org/web/20190224144312/http://www.mkyong.com/java/quartz-2-scheduler-tutorial/)
2.  [石英配置参考](http://web.archive.org/web/20190224144312/http://www.quartz-scheduler.org/documentation/quartz-2.1.x/configuration)
3.  [JSF 2.0 hello world 示例](http://web.archive.org/web/20190224144312/http://www.mkyong.com/jsf2/jsf-2-0-hello-world-example/)
4.  [XML scheduling data processor plugin JavaDoc](http://web.archive.org/web/20190224144312/http://www.quartz-scheduler.org/api/2.1.0/org/quartz/plugins/xml/XMLSchedulingDataProcessorPlugin.html)
5.  [好石英调度器工作示例](http://web.archive.org/web/20190224144312/http://www.openscope.net/2010/02/05/quartz-scheduled-jobs/)

[jsf2](http://web.archive.org/web/20190224144312/http://www.mkyong.com/tag/jsf2/) [quartz](http://web.archive.org/web/20190224144312/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190224144312/http://www.mkyong.com/tag/scheduler/)







