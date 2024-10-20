# 在 Quartz 中运行多个作业的示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/example-to-run-multiple-jobs-in-quartz/>

在这个例子中，我们展示了如何通过 Quartz APIs、Quartz XML 和 Spring 声明多个 Quartz 作业。在 Quartz 调度框架中，每个作业都将被附加到一个唯一的触发器上，并由调度程序运行。

在 Quartz 中，一个触发器不能用于多个任务。(如果这是错误的，请纠正我。)

## 1.石英原料药

创建 3 个 Quartz 的作业，JobA，JobB 和 JobC。

```java
 package com.mkyong.quartz;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class JobA implements Job {

	@Override
	public void execute(JobExecutionContext context)
		throws JobExecutionException {
		System.out.println("Job A is runing");
	}

} 
```

```java
 package com.mkyong.quartz;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class JobB implements Job {

	@Override
	public void execute(JobExecutionContext context)
		throws JobExecutionException {
		System.out.println("Job B is runing");
	}

} 
```

```java
 package com.mkyong.quartz;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class JobC implements Job {

	@Override
	public void execute(JobExecutionContext context)
		throws JobExecutionException {
		System.out.println("Job C is runing");
	}

} 
```

使用 Quartz APIs 来声明上述 3 个作业，分配给 3 个特定的触发器并调度它。

```java
 package com.mkyong.quartz;

import org.quartz.CronScheduleBuilder;
import org.quartz.JobBuilder;
import org.quartz.JobDetail;
import org.quartz.JobKey;
import org.quartz.Scheduler;
import org.quartz.Trigger;
import org.quartz.TriggerBuilder;
import org.quartz.impl.StdSchedulerFactory;

public class CronTriggerExample {
	public static void main( String[] args ) throws Exception
    {

	JobKey jobKeyA = new JobKey("jobA", "group1");
    	JobDetail jobA = JobBuilder.newJob(JobA.class)
		.withIdentity(jobKeyA).build();

    	JobKey jobKeyB = new JobKey("jobB", "group1");
    	JobDetail jobB = JobBuilder.newJob(JobB.class)
		.withIdentity(jobKeyB).build();

    	JobKey jobKeyC = new JobKey("jobC", "group1");
    	JobDetail jobC = JobBuilder.newJob(JobC.class)
		.withIdentity(jobKeyC).build();

    	Trigger trigger1 = TriggerBuilder
		.newTrigger()
		.withIdentity("dummyTriggerName1", "group1")
		.withSchedule(
			CronScheduleBuilder.cronSchedule("0/5 * * * * ?"))
		.build();

    	Trigger trigger2 = TriggerBuilder
		.newTrigger()
		.withIdentity("dummyTriggerName2", "group1")
		.withSchedule(
			CronScheduleBuilder.cronSchedule("0/5 * * * * ?"))
		.build();

    	Trigger trigger3 = TriggerBuilder
		.newTrigger()
		.withIdentity("dummyTriggerName3", "group1")
		.withSchedule(
			CronScheduleBuilder.cronSchedule("0/5 * * * * ?"))
		.build();

    	Scheduler scheduler = new StdSchedulerFactory().getScheduler();

    	scheduler.start();
    	scheduler.scheduleJob(jobA, trigger1);
    	scheduler.scheduleJob(jobB, trigger2);
    	scheduler.scheduleJob(jobC, trigger3);

    }
} 
```

输出

```java
 Job A is runing //every 5 seconds
Job B is runing
Job C is runing
Job A is runing //every 5 seconds
Job B is runing
Job C is runing 
```

 ## 2.Quartz XML 示例

XML 文件中的等效版本。确保“quartz.properties”和“quartz-config.xml”位于项目类路径中。

*文件-quartz . properties*

```java
 org.quartz.scheduler.instanceName = MyScheduler
org.quartz.threadPool.threadCount = 3
org.quartz.jobStore.class = org.quartz.simpl.RAMJobStore
org.quartz.plugin.jobInitializer.class =org.quartz.plugins.xml.XMLSchedulingDataProcessorPlugin 
org.quartz.plugin.jobInitializer.fileNames = quartz-config.xml 
org.quartz.plugin.jobInitializer.failOnFileNotFound = true 
```

*文件–quartz-config . XML*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<job-scheduling-data

	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.quartz-scheduler.org/xml/JobSchedulingData 
        http://www.quartz-scheduler.org/xml/job_scheduling_data_1_8.xsd"
	version="1.8">

	<schedule>
		<job>
			<name>JobA</name>
			<group>GroupDummy</group>
			<description>This is Job A</description>
			<job-class>com.mkyong.quartz.JobA</job-class>
		</job>

		<trigger>
			<cron>
				<name>dummyTriggerNameA</name>
				<job-name>JobA</job-name>
				<job-group>GroupDummy</job-group>
				<!-- It will run every 5 seconds -->
				<cron-expression>0/5 * * * * ?</cron-expression>
			</cron>
		</trigger>
	</schedule>

	<schedule>
		<job>
			<name>JobB</name>
			<group>GroupDummy</group>
			<description>This is Job B</description>
			<job-class>com.mkyong.quartz.JobB</job-class>
		</job>

		<trigger>
			<cron>
				<name>dummyTriggerNameB</name>
				<job-name>JobB</job-name>
				<job-group>GroupDummy</job-group>
				<!-- It will run every 5 seconds -->
				<cron-expression>0/5 * * * * ?</cron-expression>
			</cron>
		</trigger>
	</schedule>
</job-scheduling-data> 
```

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

 ## 3.春天的例子

春天的对等版本。

```java
 package com.mkyong.job;

import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;
import org.springframework.scheduling.quartz.QuartzJobBean;

public class JobA extends QuartzJobBean {

	@Override
	protected void executeInternal(JobExecutionContext arg0)
		throws JobExecutionException {
		System.out.println("Job A is runing");
	}

} 
```

```java
 package com.mkyong.job;

import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;
import org.springframework.scheduling.quartz.QuartzJobBean;

public class JobB extends QuartzJobBean {

	@Override
	protected void executeInternal(JobExecutionContext arg0)
		throws JobExecutionException {
		System.out.println("Job B is runing");

	}

} 
```

```java
 package com.mkyong.job;

import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;
import org.springframework.scheduling.quartz.QuartzJobBean;

public class JobC extends QuartzJobBean {

	@Override
	protected void executeInternal(JobExecutionContext arg0)
		throws JobExecutionException {
		System.out.println("Job C is runing");

	}

} 
```

*文件:Spring-quartz . XML*–在 Spring XML bean 配置文件中声明作业和触发器。

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

	<bean id="jobA" class="com.mkyong.job.JobA" />
	<bean id="jobB" class="com.mkyong.job.JobB" />
	<bean id="jobC" class="com.mkyong.job.JobC" />

	<!-- Quartz Job -->
	<bean name="JobA" class="org.springframework.scheduling.quartz.JobDetailBean">
		<property name="jobClass" value="com.mkyong.job.JobA" />
	</bean>

	<bean name="JobB" class="org.springframework.scheduling.quartz.JobDetailBean">
		<property name="jobClass" value="com.mkyong.job.JobB" />
	</bean>

	<bean name="JobC" class="org.springframework.scheduling.quartz.JobDetailBean">
		<property name="jobClass" value="com.mkyong.job.JobC" />
	</bean>

	<!-- Cron Trigger, run every 5 seconds -->
	<bean id="cronTriggerJobA" 
                class="org.springframework.scheduling.quartz.CronTriggerBean">
		<property name="jobDetail" ref="JobA" />
		<property name="cronExpression" value="0/5 * * * * ?" />
	</bean>

	<bean id="cronTriggerJobB" 
                class="org.springframework.scheduling.quartz.CronTriggerBean">
		<property name="jobDetail" ref="JobB" />
		<property name="cronExpression" value="0/5 * * * * ?" />
	</bean>

	<bean id="cronTriggerJobC" 
                class="org.springframework.scheduling.quartz.CronTriggerBean">
		<property name="jobDetail" ref="JobC" />
		<property name="cronExpression" value="0/5 * * * * ?" />
	</bean>

	<bean class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
		<property name="triggers">
			<list>
				<ref bean="cronTriggerJobA" />
				<ref bean="cronTriggerJobB" />
				<ref bean="cronTriggerJobC" />
			</list>
		</property>
	</bean>

</beans> 
```

运行它

```java
 package com.mkyong.common;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
	public static void main(String[] args) throws Exception {
		new ClassPathXmlApplicationContext("Spring-Quartz.xml");

	}
} 
```

输出。

```java
 INFO: Starting beans in phase 2147483647
Jul 30, 2012 10:38:13 PM org.springframework.scheduling.quartz.SchedulerFactoryBean startScheduler
INFO: Starting Quartz Scheduler now
Job A is runing
Job B is runing
Job C is runing
Job A is runing
Job B is runing
Job C is runing 
```

## 下载源代码

Download it – [Multiple-Jobs-in-Quartz-Spring-Example.zip](http://web.archive.org/web/20190225101544/http://www.mkyong.com/wp-content/uploads/2012/07/Multiple-Jobs-in-Quartz-Spring-Example.zip) (25kb)

## 参考

1.  [使用带弹簧的石英](http://web.archive.org/web/20190225101544/http://static.springsource.org/spring/docs/3.0.5.RELEASE/reference/scheduling.html#scheduling-quartz)

[quartz](http://web.archive.org/web/20190225101544/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190225101544/http://www.mkyong.com/tag/scheduler/)







