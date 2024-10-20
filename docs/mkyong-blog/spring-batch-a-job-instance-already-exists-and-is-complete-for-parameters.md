# Spring Batch:作业实例已经存在，并且对于参数={}是完整的

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-batch/spring-batch-a-job-instance-already-exists-and-is-complete-for-parameters/>

使用 Spring Batch 2.2.0.RELEASE，并使用 Spring Scheduler 启动作业。

CustomJobLauncher.java

```java
 package com.mkyong.batch;

import org.springframework.batch.core.Job;
import org.springframework.batch.core.JobExecution;
import org.springframework.batch.core.JobParameters;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class CustomJobLauncher {

	@Autowired
	JobLauncher jobLauncher;

	@Autowired
	Job job;

	public void run() {

	  try {

		JobExecution execution = jobLauncher.run(job, new JobParameters());
		System.out.println("Exit Status : " + execution.getStatus());

	  } catch (Exception e) {
		e.printStackTrace();
	  }

	}

} 
```

job-config.xml

```java
 <bean id="customJobLauncher" class="com.mkyong.batch.CustomJobLauncher" />

  <task:scheduled-tasks>
	<task:scheduled ref="customJobLauncher" method="run" fixed-delay="10000" />
  </task:scheduled-tasks> 
```

## 问题

批处理作业仅在第一次运行时成功，当它第二次启动时(10 秒后)，它会提示以下错误消息。

```java
 org.springframework.batch.core.repository.JobInstanceAlreadyCompleteException: 
	A job instance already exists and is complete for parameters={}.  
        If you want to run this job again, change the parameters.
	at org.springframework.batch.core.repository.support.SimpleJobRepository.createJobExecution(SimpleJobRepository.java:126)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25) 
```

 ## 解决办法

参考上面的错误消息"**如果您想再次运行该任务，请更改参数。**“公式是`JobInstance = JobParameters + Job`。如果您没有任何用于`JobParameters`的参数，只需传递一个当前时间作为参数来创建一个新的`JobInstance`。举个例子，

CustomJobLauncher.java

```java
 //...

@Component
public class CustomJobLauncher {

	@Autowired
	JobLauncher jobLauncher;

	@Autowired
	Job job;

	public void run() {

	  try {
		JobParameters jobParameters = 
		  new JobParametersBuilder()
		  .addLong("time",System.currentTimeMillis()).toJobParameters();

		JobExecution execution = jobLauncher.run(job, jobParameters);
		System.out.println("Exit Status : " + execution.getStatus());

	  } catch (Exception e) {
		e.printStackTrace();
	  }

	}

} 
```

 ## 参考

1.  [Spring Batch:配置和运行作业](http://web.archive.org/web/20190303052552/http://static.springsource.org/spring-batch/reference/html/configureJob.html)
2.  [如何创建新的作业实例](http://web.archive.org/web/20190303052552/http://forum.springsource.org/showthread.php?58319-How-to-create-new-job-instance)

[spring batch](http://web.archive.org/web/20190303052552/http://www.mkyong.com/tag/spring-batch/)







