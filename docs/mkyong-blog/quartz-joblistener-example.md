# Quartz 2 作业监听器示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/quartz-joblistener-example/>

在本教程中，我们将向您展示如何创建一个`JobListener`，来跟踪正在运行的作业状态，比如作业何时完成。

*附注:本示例使用石英 2.1.5* 进行测试

## 1.石英工作

Job，打印一条简单的消息，并抛出一个`JobExecutionException`进行测试。

*文件:HelloJob.java*

```java
 package com.mkyong.quartz;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class HelloJob implements Job
{
	public void execute(JobExecutionContext context)
	throws JobExecutionException {

		System.out.println("Hello Quartz! 123");	

		//Throw exception for testing
		throw new JobExecutionException("Testing Exception");
	}

} 
```

 ## 2.作业监听器

要创建 JobListener，只需实现`JobListener`接口，并覆盖所有接口的方法。

*文件:HelloJobListener.java*

```java
 package com.mkyong.quartz.listener;

import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;
import org.quartz.JobListener;

public class HelloJobListener implements JobListener {

	public static final String LISTENER_NAME = "dummyJobListenerName";

	@Override
	public String getName() {
		return LISTENER_NAME; //must return a name
	}

	// Run this if job is about to be executed.
	@Override
	public void jobToBeExecuted(JobExecutionContext context) {

		String jobName = context.getJobDetail().getKey().toString();
		System.out.println("jobToBeExecuted");
		System.out.println("Job : " + jobName + " is going to start...");

	}

	// No idea when will run this?
	@Override
	public void jobExecutionVetoed(JobExecutionContext context) {
		System.out.println("jobExecutionVetoed");
	}

	//Run this after job has been executed
	@Override
	public void jobWasExecuted(JobExecutionContext context,
			JobExecutionException jobException) {
		System.out.println("jobWasExecuted");

		String jobName = context.getJobDetail().getKey().toString();
		System.out.println("Job : " + jobName + " is finished...");

		if (!jobException.getMessage().equals("")) {
			System.out.println("Exception thrown by: " + jobName
				+ " Exception: " + jobException.getMessage());
		}

	}

} 
```

**Note**
No idea what is “jobExecutionVetoed” and when will it triggered? Do comment if you know this, thanks. ## 3.克朗触发器

示例将上述`HelloJobListener`附加到调度程序，并监控作业的状态。

*文件:CronTriggerExample.java*

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
import org.quartz.impl.matchers.KeyMatcher;

import com.mkyong.quartz.listener.HelloJobListener;

public class CronTriggerExample {
    public static void main( String[] args ) throws Exception
    {

	JobKey jobKey = new JobKey("dummyJobName", "group1");
    	JobDetail job = JobBuilder.newJob(HelloJob.class)
		.withIdentity(jobKey).build();

    	Trigger trigger = TriggerBuilder
		.newTrigger()
		.withIdentity("dummyTriggerName", "group1")
		.withSchedule(
			CronScheduleBuilder.cronSchedule("0/5 * * * * ?"))
		.build();

    	Scheduler scheduler = new StdSchedulerFactory().getScheduler();

    	//Listener attached to jobKey
    	scheduler.getListenerManager().addJobListener(
    		new HelloJobListener(), KeyMatcher.keyEquals(jobKey)
    	);

    	//Listener attached to group named "group 1" only.
    	//scheduler.getListenerManager().addJobListener(
    	//	new HelloJobListener(), GroupMatcher.jobGroupEquals("group1")
    	//);

    	scheduler.start();
    	scheduler.scheduleJob(job, trigger);

    }
} 
```

运行`CronTriggerExample.java`，这里是输出。

```java
 jobToBeExecuted
Job : group1.dummyJobName is going to start...
Hello Quartz! 123
jobWasExecuted
Job : group1.dummyJobName is started and finished...
Exception thrown by: group1.dummyJobName Exception: Testing Exception

jobToBeExecuted
Job : group1.dummyJobName is going to start...
Hello Quartz! 123
jobWasExecuted
Job : group1.dummyJobName is started and finished...
Exception thrown by: group1.dummyJobName Exception: Testing Exception 
```

## 下载源代码

Download it – [Quartz2-JobListener-Example.zip](http://web.archive.org/web/20190226180851/http://www.mkyong.com/wp-content/uploads/2012/07/Quartz2-JobListener-Example.zip) (13 KB)

## 参考

1.  [石英官网](http://web.archive.org/web/20190226180851/http://www.quartz-scheduler.org/)
2.  [Quartz 2 JobListener 文档](http://web.archive.org/web/20190226180851/http://quartz-scheduler.org/documentation/quartz-2.1.x/tutorials/tutorial-lesson-07)
3.  [石英 2 hello world 示例](http://web.archive.org/web/20190226180851/http://www.mkyong.com/java/quartz-2-scheduler-tutorial/)

[quartz](http://web.archive.org/web/20190226180851/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190226180851/http://www.mkyong.com/tag/scheduler/)







