# 如何手动触发 Quartz 作业(JSF 2 示例)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/jsf2/how-to-trigger-a-quartz-job-manually-jsf-2-example/>

在 Quartz 中，您可以通过以下模式手动触发作业:

```java
 JobKey jobKey = new JobKey(jobName, jobGroup);
scheduler.triggerJob(jobKey); //trigger a job by jobkey 
```

在本教程中，我们将向您展示一个 JSF 2 web 应用程序，显示`dataTable`上的所有 Quartz 作业，并允许用户点击一个链接来手动启动作业。

使用的工具:

1.  JSF 2.1.11
2.  石英 2.1.5
3.  Eclipse 4.2
4.  maven3
5.  在 Tomcat 6 和 7 上测试

## 1.项目目录

最终项目目录。

![final project directory](img/563a13e9b96e899fce291e2031a3757d.png "project-directory") ## 2.项目依赖性

本教程的所有依赖项。

*文件:pom.xml*

```java
 <project ...>

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

		<!-- solve method not found error in tomcat --> 
		<dependency>
			<groupId>org.glassfish.web</groupId>
			<artifactId>el-impl</artifactId>
			<version>2.2</version>
		</dependency>

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

		<dependency>
			<groupId>javax.transaction</groupId>
			<artifactId>jta</artifactId>
			<version>1.1</version>
		</dependency>

	</dependencies>

</project> 
```

 ## 3.石英工作

通过`web.xml`中的监听器，创造两份工作并与 JSF 整合。

```java
 package com.mkyong.jobs;

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
 package com.mkyong.jobs;

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

*文件:quartz.properties*

```java
 org.quartz.scheduler.instanceName = MyScheduler
org.quartz.threadPool.threadCount = 3
org.quartz.jobStore.class = org.quartz.simpl.RAMJobStore
org.quartz.plugin.jobInitializer.class =org.quartz.plugins.xml.XMLSchedulingDataProcessorPlugin 
org.quartz.plugin.jobInitializer.fileNames = quartz-config.xml 
org.quartz.plugin.jobInitializer.failOnFileNotFound = true 
```

*文件:quartz.config*

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
			<job-class>com.mkyong.jobs.JobA</job-class>
		</job>

		<trigger>
			<cron>
				<name>dummyTriggerNameA</name>
				<job-name>JobA</job-name>
				<job-group>GroupDummy</job-group>
				<!-- It will run every 30 seconds -->
				<cron-expression>0/30 * * * * ?</cron-expression>
			</cron>
		</trigger>
	</schedule>

	<schedule>
		<job>
			<name>JobB</name>
			<group>GroupDummy</group>
			<description>This is Job B</description>
			<job-class>com.mkyong.jobs.JobB</job-class>
		</job>

		<trigger>
			<cron>
				<name>dummyTriggerNameB</name>
				<job-name>JobB</job-name>
				<job-group>GroupDummy</job-group>
				<!-- It will run every 30 seconds -->
				<cron-expression>0/30 * * * * ?</cron-expression>
			</cron>
		</trigger>
	</schedule>
</job-scheduling-data> 
```

*文件:web.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

	xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="WebApp_ID" version="2.5">

	<display-name>JavaServerFaces</display-name>

	<!-- Change to "Production" when you are ready to deploy -->
	<context-param>
		<param-name>javax.faces.PROJECT_STAGE</param-name>
		<param-value>Development</param-value>
	</context-param>

	<!-- Welcome page -->
	<welcome-file-list>
		<welcome-file>faces/welcome.xhtml</welcome-file>
	</welcome-file-list>

	<!-- JSF mapping -->
	<servlet>
		<servlet-name>Faces Servlet</servlet-name>
		<servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<!-- Map these files with JSF -->
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>/faces/*</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.jsf</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.faces</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>*.xhtml</url-pattern>
	</servlet-mapping>

	<listener>
		<listener-class>
			org.quartz.ee.servlet.QuartzInitializerListener
		</listener-class>
	</listener>

</web-app> 
```

## 4.JSF 豆

一个 JSF 比恩为后来的`dataTable`提供数据。在构造函数中，获取所有现有的作业并添加到一个列表中，然后返回它。

```java
 package com.mkyong.scheduler;

import java.io.Serializable;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.faces.context.FacesContext;
import javax.servlet.ServletContext;

import org.quartz.JobKey;
import org.quartz.Scheduler;
import org.quartz.SchedulerException;
import org.quartz.Trigger;
import org.quartz.ee.servlet.QuartzInitializerListener;
import org.quartz.impl.StdSchedulerFactory;
import org.quartz.impl.matchers.GroupMatcher;

@ManagedBean(name = "scheduler")
@SessionScoped
public class SchedulerBean implements Serializable {

	private static final long serialVersionUID = 1L;

	private Scheduler scheduler;

	private List<QuartzJob> quartzJobList = new ArrayList<QuartzJob>();

	public SchedulerBean() throws SchedulerException {

	  ServletContext servletContext = (ServletContext) FacesContext
		.getCurrentInstance().getExternalContext().getContext();

	  //Get QuartzInitializerListener 
	  StdSchedulerFactory stdSchedulerFactory = (StdSchedulerFactory) servletContext
		.getAttribute(QuartzInitializerListener.QUARTZ_FACTORY_KEY);

	  scheduler = stdSchedulerFactory.getScheduler();

	  // loop jobs by group
	  for (String groupName : scheduler.getJobGroupNames()) {

		// get jobkey
		for (JobKey jobKey : scheduler.getJobKeys(GroupMatcher
			.jobGroupEquals(groupName))) {

			String jobName = jobKey.getName();
			String jobGroup = jobKey.getGroup();

			// get job's trigger
			List<Trigger> triggers = (List<Trigger>) scheduler
				.getTriggersOfJob(jobKey);
			Date nextFireTime = triggers.get(0).getNextFireTime();

			quartzJobList.add(new QuartzJob(jobName, jobGroup, nextFireTime));

		}

	  }

	}

	//trigger a job
	public void fireNow(String jobName, String jobGroup)
		throws SchedulerException {

		JobKey jobKey = new JobKey(jobName, jobGroup);
		scheduler.triggerJob(jobKey);

	}

	public List<QuartzJob> getQuartzJobList() {

		return quartzJobList;

	}

	public static class QuartzJob {

		private static final long serialVersionUID = 1L;

		String jobName;
		String jobGroup;
		Date nextFireTime;

		public QuartzJob(String jobName, String jobGroup, Date nextFireTime) {

			this.jobName = jobName;
			this.jobGroup = jobGroup;
			this.nextFireTime = nextFireTime;
		}

		public String getJobName() {
			return jobName;
		}

		public void setJobName(String jobName) {
			this.jobName = jobName;
		}

		public String getJobGroup() {
			return jobGroup;
		}

		public void setJobGroup(String jobGroup) {
			this.jobGroup = jobGroup;
		}

		public Date getNextFireTime() {
			return nextFireTime;
		}

		public void setNextFireTime(Date nextFireTime) {
			this.nextFireTime = nextFireTime;
		}

	}

} 
```

## 5.JSF 页面

一个网页，通过 EL `#{scheduler.quartzJobList}`获取所有已有的作业，通过`dataTable`组件显示。当单击“fireNow”链接时，它将立即启动指定的作业。

*文件:welcome.xhtml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html 
	xmlns:h="http://java.sun.com/jsf/html"
	xmlns:f="http://java.sun.com/jsf/core"
	xmlns:ui="http://java.sun.com/jsf/facelets">
<h:head>
	<h:outputStylesheet library="css" name="table-style.css" />
</h:head>
<h:body>

  <h1>All Quartz Jobs</h1>
  <h:form>
	<h:dataTable value="#{scheduler.quartzJobList}" var="quartz"
		styleClass="quartz-table" headerClass="quartz-table-header"
		rowClasses="quartz-table-odd-row,quartz-table-even-row">

		<h:column>
		  <!-- column header -->
		  <f:facet name="header">Job Name</f:facet>
		  <!-- row record -->
    		  #{quartz.jobName}
    		</h:column>

		<h:column>
		  <f:facet name="header">Job Group</f:facet>
    		  #{quartz.jobGroup}
    		</h:column>

		<h:column>
		  <f:facet name="header">Next Fire Time</f:facet>
		  <h:outputText value="#{quartz.nextFireTime}">
		      <f:convertDateTime pattern="dd.MM.yyyy HH:mm" />
		  </h:outputText>
		</h:column>

		<h:column>
		  <f:facet name="header">Action</f:facet>
		  <h:commandLink value="Fire Now"
		     action="#{scheduler.fireNow(quartz.jobName, quartz.jobGroup)}" />
		</h:column>

	   </h:dataTable>
	</h:form>
</h:body>
</html> 
```

*文件:table-style.css*

```java
 .quartz-table{   
	border-collapse:collapse;
}

.quartz-table-header{
	text-align:center;
	background:none repeat scroll 0 0 #E5E5E5;
	border-bottom:1px solid #BBBBBB;
	padding:16px;
}

.quartz-table-odd-row{
	text-align:center;
	background:none repeat scroll 0 0 #FFFFFFF;
	border-top:1px solid #BBBBBB;
	padding:20px;
}

.quartz-table-even-row{
	text-align:center;
	background:none repeat scroll 0 0 #F9F9F9;
	border-top:1px solid #BBBBBB;
	padding:20px;
} 
```

## 6.演示

请参见最终输出。*http://localhost:8080/Java server faces/*

![list all quartz jobs](img/e63bed9e4774753fb3bddd6fb9f91416.png "all-quartz-jobs")

## 下载源代码

Download it – [JSF-Quartz-Trigger-Job-Manually.zip](http://web.archive.org/web/20190219073326/http://www.mkyong.com/wp-content/uploads/2012/08/JSF-Quartz-Trigger-Job-Manually.zip) (28 kb)

## 参考

1.  [Quartz 2 管理员页面示例](http://web.archive.org/web/20190219073326/http://neopatel.blogspot.com/2010/02/quartz-admin-jsp.html)
2.  [石英 2 + JSF 2 集成示例](http://web.archive.org/web/20190219073326/http://www.mkyong.com/jsf2/jsf-2-quartz-2-example/)
3.  jquartzinitializer listener JavaDoc
4.  [石英中的多项工作](http://web.archive.org/web/20190219073326/http://www.mkyong.com/java/example-to-run-multiple-jobs-in-quartz/)

[jsf2](http://web.archive.org/web/20190219073326/http://www.mkyong.com/tag/jsf2/) [quartz](http://web.archive.org/web/20190219073326/http://www.mkyong.com/tag/quartz/) [scheduler](http://web.archive.org/web/20190219073326/http://www.mkyong.com/tag/scheduler/)







