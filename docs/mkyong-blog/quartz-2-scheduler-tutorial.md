# Quartz 2 调度程序教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/quartz-2-scheduler-tutorial/>

[Quartz](http://web.archive.org/web/20210506151032/http://www.quartz-scheduler.org/) ，企业调度器作业框架，帮助 Java 应用程序调度作业/任务在指定的日期和时间运行。

本教程向您展示了如何使用最新的 Quartz library 2.1.5 开发一个调度作业。

**Note**
Quartz 2 involves significant API changed, read this for [older Quartz 1.6.3 example](http://web.archive.org/web/20210506151032/http://www.mkyong.com/java/quartz-scheduler-example/).

## 1.下载 Quartz

可以从[官网](http://web.archive.org/web/20210506151032/http://www.quartz-scheduler.org/)或者 Maven 中央知识库获取石英库

*文件:pom.xml*

```java
 <dependencies>
	<dependency>
		<groupId>org.quartz-scheduler</groupId>
		<artifactId>quartz</artifactId>
		<version>2.1.5</version>
	</dependency>
</dependencies> 
```

**Note**
To deploy Quartz on Application server like JBoss, oracle or weblogic, you may need addition Quartz dependency, read this [guide](http://web.archive.org/web/20210506151032/http://quartz-scheduler.org/downloads/catalog).

## 2.石英工作

Quartz 作业定义了你要运行什么？

*文件:HelloJob*

```java
 package com.mkyong.common;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class HelloJob implements Job
{
	public void execute(JobExecutionContext context)
	throws JobExecutionException {

		System.out.println("Hello Quartz!");	

	}

} 
```

## 3.石英触发器

石英触发器定义了石英什么时候会运行你上面的石英的工作？

像旧的 Quartz 一样，Quartz 2 中仍然有两种类型的触发器，但是 API 发生了变化:

*   simple trigger–允许设置开始时间、结束时间和重复间隔。
*   CronTrigger 允许 Unix cron 表达式指定运行作业的日期和时间。

简单触发器–每 5 秒运行一次。

```java
 Trigger trigger = TriggerBuilder
	.newTrigger()
	.withIdentity("dummyTriggerName", "group1")
	.withSchedule(
	    SimpleScheduleBuilder.simpleSchedule()
		.withIntervalInSeconds(5).repeatForever())
	.build(); 
```

cron trigger–每 5 秒运行一次。

```java
 Trigger trigger = TriggerBuilder
	.newTrigger()
	.withIdentity("dummyTriggerName", "group1")
	.withSchedule(
		CronScheduleBuilder.cronSchedule("0/5 * * * * ?"))
	.build(); 
```

**Note**
Read this [official documentation](http://web.archive.org/web/20210506151032/http://quartz-scheduler.org/documentation/quartz-2.x/tutorials/) for more Quartz 2 trigger examples.

## 4.调度程序

调度器类将“**作业**和“**触发器**链接在一起并执行。

```java
 Scheduler scheduler = new StdSchedulerFactory().getScheduler();
    	scheduler.start();
    	scheduler.scheduleJob(job, trigger); 
```

## 5.完整示例

Quartz 2 带有 SimpleTrigger 和 CronTrigger 的完整示例。

**简单触发器示例**–每隔 5 秒运行一次。

```java
 package com.mkyong.quartz;

import org.quartz.JobBuilder;
import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.SimpleScheduleBuilder;
import org.quartz.Trigger;
import org.quartz.TriggerBuilder;
import org.quartz.impl.StdSchedulerFactory;

public class SimpleTriggerExample {
	public static void main(String[] args) throws Exception {

		// Quartz 1.6.3
		// JobDetail job = new JobDetail();
		// job.setName("dummyJobName");
		// job.setJobClass(HelloJob.class);

		JobDetail job = JobBuilder.newJob(HelloJob.class)
			.withIdentity("dummyJobName", "group1").build();

                //Quartz 1.6.3
		// SimpleTrigger trigger = new SimpleTrigger();
		// trigger.setStartTime(new Date(System.currentTimeMillis() + 1000));
		// trigger.setRepeatCount(SimpleTrigger.REPEAT_INDEFINITELY);
		// trigger.setRepeatInterval(30000);

		// Trigger the job to run on the next round minute
		Trigger trigger = TriggerBuilder
			.newTrigger()
			.withIdentity("dummyTriggerName", "group1")
			.withSchedule(
				SimpleScheduleBuilder.simpleSchedule()
					.withIntervalInSeconds(5).repeatForever())
			.build();

		// schedule it
		Scheduler scheduler = new StdSchedulerFactory().getScheduler();
		scheduler.start();
		scheduler.scheduleJob(job, trigger);

	}
} 
```

**CronTrigger 示例**–相同，每 5 秒运行一次作业。

```java
 package com.mkyong.quartz;

import org.quartz.CronScheduleBuilder;
import org.quartz.JobBuilder;
import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.Trigger;
import org.quartz.TriggerBuilder;
import org.quartz.impl.StdSchedulerFactory;

public class CronTriggerExample 
{
    public static void main( String[] args ) throws Exception
    {
    	//Quartz 1.6.3
    	//JobDetail job = new JobDetail();
    	//job.setName("dummyJobName");
    	//job.setJobClass(HelloJob.class);    	
    	JobDetail job = JobBuilder.newJob(HelloJob.class)
		.withIdentity("dummyJobName", "group1").build();

	//Quartz 1.6.3
    	//CronTrigger trigger = new CronTrigger();
    	//trigger.setName("dummyTriggerName");
    	//trigger.setCronExpression("0/5 * * * * ?");

    	Trigger trigger = TriggerBuilder
		.newTrigger()
		.withIdentity("dummyTriggerName", "group1")
		.withSchedule(
			CronScheduleBuilder.cronSchedule("0/5 * * * * ?"))
		.build();

    	//schedule it
    	Scheduler scheduler = new StdSchedulerFactory().getScheduler();
    	scheduler.start();
    	scheduler.scheduleJob(job, trigger);

    }
} 
```

## 下载源代码

Download it – [Quartz2Example.zip](http://web.archive.org/web/20210506151032/http://www.mkyong.com/wp-content/uploads/2012/07/Quartz2Example.zip) (10kb)

## 参考

1.  [石英官网](http://web.archive.org/web/20210506151032/http://www.quartz-scheduler.org/)
2.  [石英官方文档](http://web.archive.org/web/20210506151032/http://quartz-scheduler.org/documentation/quartz-2.x/tutorials/)
3.  [启动石英 2 教程](http://web.archive.org/web/20210506151032/https://javaeenotes.blogspot.com/2011/09/kick-start-quartz-2-tutorial.html)
4.  [Unix CRON 表达式](http://web.archive.org/web/20210506151032/https://en.wikipedia.org/wiki/CRON_expression)

Tags : [quartz](http://web.archive.org/web/20210506151032/https://mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20210506151032/https://mkyong.com/tag/scheduler/) [tutorials](http://web.archive.org/web/20210506151032/https://mkyong.com/tag/tutorials/)<input type="hidden" id="mkyong-current-postId" value="11127">