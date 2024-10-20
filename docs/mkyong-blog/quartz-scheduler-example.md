# Quartz 1.6 调度器教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/quartz-scheduler-example/>

Quartz 是一个强大的高级调度框架，帮助 Java 开发者调度一个任务在指定的日期和时间运行。

本教程向您展示了如何使用 Quartz 1.6.3 开发一个调度作业。

**Note**
This example is a bit outdate, unless you are still using the old Quartz 1.6.3 library, otherwise, you may interest of this latest [Quartz 2.1.5 example](http://web.archive.org/web/20190216083728/http://www.mkyong.com/java/quartz-2-scheduler-tutorial/).

## 1.下载 Quartz

可以从[官网](http://web.archive.org/web/20190216083728/http://www.quartz-scheduler.org/)或者 Maven 中央知识库获取石英库

文件:pom.xml

```java
 <dependencies>

		<!-- Quartz API -->
		<dependency>
			<groupId>opensymphony</groupId>
			<artifactId>quartz</artifactId>
			<version>1.6.3</version>
		</dependency>

		<dependency>
			<groupId>commons-collections</groupId>
			<artifactId>commons-collections</artifactId>
			<version>3.2.1</version>
		</dependency>

		<dependency>
			<groupId>org.apache.directory.studio</groupId>
			<artifactId>org.apache.commons.logging</artifactId>
			<version>1.1.1</version>
		</dependency>

	</dependencies> 
```

 ## 2.石英工作

Quartz 作业定义了你要运行什么？

文件:HelloJob

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

有两种类型的石英触发器:

*   simple trigger–允许设置开始时间、结束时间和重复间隔。
*   CronTrigger 允许 Unix cron 表达式指定运行作业的日期和时间。

Unix cron expression
The Unix cron expression is highly flexible and powerful, you can learn and see many cron expression examples in following websites.

1.  [http://en . Wikipedia . org/wiki/cron _ expression](http://web.archive.org/web/20190216083728/http://en.wikipedia.org/wiki/CRON_expression)
2.  [http://www.quartz-scheduler.org/docs/examples/Example3.html](http://web.archive.org/web/20190216083728/http://www.quartz-scheduler.org/docs/examples/Example3.html)

simple trigger–每 30 秒运行一次。

```java
 SimpleTrigger trigger = new SimpleTrigger();
    	trigger.setName("dummyTriggerName");
    	trigger.setStartTime(new Date(System.currentTimeMillis() + 1000));
    	trigger.setRepeatCount(SimpleTrigger.REPEAT_INDEFINITELY);
    	trigger.setRepeatInterval(30000); 
```

cron trigger–每 30 秒运行一次。

```java
 CronTrigger trigger = new CronTrigger();
    	trigger.setName("dummyTriggerName");
    	trigger.setCronExpression("0/30 * * * * ?"); 
```

## 4.调度程序

调度器类将“**作业**和“**触发器**链接在一起并执行。

```java
 Scheduler scheduler = new StdSchedulerFactory().getScheduler();
    	scheduler.start();
    	scheduler.scheduleJob(job, trigger); 
```

## 5.完整示例

下面是通过 SimpleTrigger 和 CronTrigger 使用 Quartz 的两个完整示例。

**SimpleTrigger 示例**
第一次执行时延时 1 秒运行 30 秒。

```java
 package com.mkyong.common;

import java.util.Date;

import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.SimpleTrigger;
import org.quartz.impl.StdSchedulerFactory;

public class SimpleTriggerExample 
{
    public static void main( String[] args ) throws Exception
    {
       	JobDetail job = new JobDetail();
    	job.setName("dummyJobName");
    	job.setJobClass(HelloJob.class);

    	//configure the scheduler time
    	SimpleTrigger trigger = new SimpleTrigger();
    	trigger.setStartTime(new Date(System.currentTimeMillis() + 1000));
    	trigger.setRepeatCount(SimpleTrigger.REPEAT_INDEFINITELY);
    	trigger.setRepeatInterval(30000);

    	//schedule it
    	Scheduler scheduler = new StdSchedulerFactory().getScheduler();
    	scheduler.start();
    	scheduler.scheduleJob(job, trigger);

    }
} 
```

**CronTrigger 示例**
相同，每 30 秒运行一次作业。

```java
 package com.mkyong.common;

import org.quartz.CronTrigger;
import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.impl.StdSchedulerFactory;

public class CronTriggerExample 
{
    public static void main( String[] args ) throws Exception
    {

    	JobDetail job = new JobDetail();
    	job.setName("dummyJobName");
    	job.setJobClass(HelloJob.class);

    	CronTrigger trigger = new CronTrigger();
    	trigger.setName("dummyTriggerName");
    	trigger.setCronExpression("0/30 * * * * ?");

    	//schedule it
    	Scheduler scheduler = new StdSchedulerFactory().getScheduler();
    	scheduler.start();
    	scheduler.scheduleJob(job, trigger);

    }
} 
```

## 下载源代码

Download it – [QuartzExample.zip](http://web.archive.org/web/20190216083728/http://www.mkyong.com/wp-content/uploads/2010/04/QuartzExample.zip) (14kb)

## 参考

1.  [石英官网](http://web.archive.org/web/20190216083728/http://www.quartz-scheduler.org/)

[java](http://web.archive.org/web/20190216083728/http://www.mkyong.com/tag/java/) [quartz](http://web.archive.org/web/20190216083728/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190216083728/http://www.mkyong.com/tag/scheduler/)







