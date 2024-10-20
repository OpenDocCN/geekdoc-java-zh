# Struts 1 + Spring 2.5.6 + Quartz 1.6 调度器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-spring-quartz-scheduler-integration-example/>

在本教程中，您将集成 Struts + Spring + Quartz 框架来执行调度任务。Spring 提供了现成的解决方案来轻松集成支柱和石英。关系如下:

```java
 Struts <--(Plug-In)--> Spring <--(Spring-Helper)--> Quartz <---> Scheduler task 
```

使用的工具:

1.  Struts 1.3.10
2.  弹簧 2.5.6
3.  石英

## 1.调度程序任务

创建一个调度器任务，`printMessage()`就是你想要调度的方法。

*文件:SchedulerTask.java*

```java
 package com.mkyong.common.quartz;

public class SchedulerTask 
{
   public void printMessage() {
	System.out.println("Struts + Spring + Quartz integration example ~");
   }
} 
```

 ## 2.调度程序作业

要集成 Spring 和 Quartz，创建一个扩展 Spring 的`QuartzJobBean`的 SchedulerJob，而不是 Quartz Job 类。

*文件:SchedulerJob.java*

```java
 package com.mkyong.common.quartz;

import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;
import org.springframework.scheduling.quartz.QuartzJobBean;

public class SchedulerJob extends QuartzJobBean
{
	private SchedulerTask schedulerTask;

	public void setSchedulerTask(SchedulerTask schedulerTask) {
		this.schedulerTask = schedulerTask;
	}

	protected void executeInternal(JobExecutionContext context)
	throws JobExecutionException {

		schedulerTask.printMessage();

	}
} 
```

 ## 3.春天的石英助手

Spring 附带了许多 Quartz 助手类来简化整个 Quartz 调度程序流程——调度程序、Trigget、作业和作业细节。

*文件:spring-scheduler.xml*

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

  <!-- Scheduler task -->
  <bean name="schedulerTask" class="com.mkyong.common.quartz.SchedulerTask" />

   <!-- Scheduler job -->
   <bean name="schedulerJob" 
     class="org.springframework.scheduling.quartz.JobDetailBean">

     <property name="jobClass" 
           value="com.mkyong.common.quartz.SchedulerJob" />

     <property name="jobDataAsMap">
	<map>
	   <entry key="schedulerTask" value-ref="schedulerTask" />
	 </map>
      </property>

   </bean>

   <!-- Cron Trigger -->
   <bean id="cronTrigger"
	class="org.springframework.scheduling.quartz.CronTriggerBean">

	<property name="jobDetail" ref="schedulerJob" />
	<property name="cronExpression" value="0/5 * * * * ?" />

   </bean>

   <!-- Scheduler -->
   <bean class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
	<property name="jobDetails">
	   <list>
	      <ref bean="schedulerJob" />
	   </list>
	</property>

	<property name="triggers">
	    <list>
		<ref bean="cronTrigger" />
	    </list>
	</property>
   </bean>

</beans> 
```

## 4.支杆

要集成 Spring 和 Struts，您需要将 Spring 的`ContextLoaderPlugIn`包含到 Struts 配置文件中。

*文件:struts-config.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

   <action-mappings>

    <action
	path="/Welcome"
	type="org.apache.struts.actions.ForwardAction"
	parameter="/pages/quartz_started.jsp"/>

   </action-mappings>

   <!-- Spring Struts plugin -->
   <plug-in className="org.springframework.web.struts.ContextLoaderPlugIn">
	<set-property property="contextConfigLocation"
	value="/WEB-INF/spring-scheduler.xml" />
    </plug-in>

</struts-config> 
```

## 5.它是如何工作的

在 Struts 初始化期间，它将通过 Spring 的`ContextLoaderPlugIn` Struts 插件启动 Spring Ioc 容器；当 Spring 初始化时，它将自动启动 Quartz 调度任务。

在本例中，`printMessage()`方法将每 5 秒执行一次。

## 下载源代码

Download it – [Struts-Spring-Quartz-Example.zip](http://web.archive.org/web/20190217081730/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-Spring-Quartz-Example.zip)

## 参考

更多的细节解释，你可以参考下面的教程

1.  [Struts + Spring 集成示例](http://web.archive.org/web/20190217081730/http://www.mkyong.com/struts/struts-spring-integration-example/)
2.  [Spring + Quartz 调度器集成示例](http://web.archive.org/web/20190217081730/http://www.mkyong.com/spring/spring-quartz-scheduler-example/)

[integration](http://web.archive.org/web/20190217081730/http://www.mkyong.com/tag/integration/) [quartz](http://web.archive.org/web/20190217081730/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190217081730/http://www.mkyong.com/tag/scheduler/) [spring](http://web.archive.org/web/20190217081730/http://www.mkyong.com/tag/spring/) [struts](http://web.archive.org/web/20190217081730/http://www.mkyong.com/tag/struts/)







