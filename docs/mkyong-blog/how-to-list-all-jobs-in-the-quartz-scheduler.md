# 如何在 Quartz 调度程序中列出所有作业

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-list-all-jobs-in-the-quartz-scheduler/>

下面是两个代码片段，向您展示如何列出所有 Quartz 作业。Quartz 2 APIs 变化很大，所以语法和 Quartz 1.x 不一样。

## 1.石英 2.1.5 示例

```java
 Scheduler scheduler = new StdSchedulerFactory().getScheduler();

   for (String groupName : scheduler.getJobGroupNames()) {

     for (JobKey jobKey : scheduler.getJobKeys(GroupMatcher.jobGroupEquals(groupName))) {

	  String jobName = jobKey.getName();
	  String jobGroup = jobKey.getGroup();

	  //get job's trigger
	  List<Trigger> triggers = (List<Trigger>) scheduler.getTriggersOfJob(jobKey);
	  Date nextFireTime = triggers.get(0).getNextFireTime(); 

		System.out.println("[jobName] : " + jobName + " [groupName] : "
			+ jobGroup + " - " + nextFireTime);

	  }

    } 
```

 ## 2.石英 1.8.6 示例

```java
 Scheduler scheduler = new StdSchedulerFactory().getScheduler();

    //loop all group
    for (String groupName : scheduler.getJobGroupNames()) {

	//loop all jobs by groupname
	for (String jobName : scheduler.getJobNames(groupName)) {

          //get job's trigger
	  Trigger[] triggers = scheduler.getTriggersOfJob(jobName,groupName);
	  Date nextFireTime = triggers[0].getNextFireTime();

	  System.out.println("[jobName] : " + jobName + " [groupName] : "
			+ groupName + " - " + nextFireTime);

	}

    } 
```

**Note**
You may also interest at this example – [list all jobs and display on JSF page](http://web.archive.org/web/20190304001256/http://www.mkyong.com/jsf2/how-to-trigger-a-quartz-job-manually-jsf-2-example/). ## 参考

1.  [石英列表工作食谱](http://web.archive.org/web/20190304001256/http://quartz-scheduler.org/documentation/quartz-2.x/cookbook/ListJobs)

[quartz](http://web.archive.org/web/20190304001256/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190304001256/http://www.mkyong.com/tag/scheduler/)







