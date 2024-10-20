# 春季批量单元测试示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-batch/spring-batch-unit-test-example/>

在本教程中，我们将向您展示如何使用 jUnit 和 TestNG 框架对 Spring 批处理作业进行单元测试。对批处理作业进行单元测试，声明`spring-batch-test.jar`、*、@autowired* 和`JobLauncherTestUtils`，启动作业或步骤，并断言执行状态。

## 1.单元测试依赖关系

为了对 Spring 批处理进行单元测试，声明了以下依赖项:

pom.xml

```java
 <!-- Spring Batch dependencies -->
	<dependency>
		<groupId>org.springframework.batch</groupId>
		<artifactId>spring-batch-core</artifactId>
		<version>2.2.0.RELEASE</version>
	</dependency>
	<dependency>
		<groupId>org.springframework.batch</groupId>
		<artifactId>spring-batch-infrastructure</artifactId>
		<version>2.2.0.RELEASE</version>
	</dependency>

	<!-- Spring Batch unit test -->	
	<dependency>
		<groupId>org.springframework.batch</groupId>
		<artifactId>spring-batch-test</artifactId>
		<version>2.2.0.RELEASE</version>
	</dependency>

	<!-- Junit -->
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.11</version>
		<scope>test</scope>
	</dependency>

	<!-- Testng -->
	<dependency>
		<groupId>org.testng</groupId>
		<artifactId>testng</artifactId>
		<version>6.8.5</version>
		<scope>test</scope>
	</dependency> 
```

 ## 2.春季批处理作业

一个简单的作业，稍后单元测试执行状态。

spring-batch-job.xml

```java
 <!-- ...... -->
    <batch:job id="testJob">
	<batch:step id="step1">
	    <batch:tasklet>
		<batch:chunk reader="xmlItemReader" 
			writer="oracleItemWriter"
			commit-interval="1">
		</batch:chunk>
	    </batch:tasklet>
	</batch:step>
    </batch:job> 
```

 ## 3.jUnit 示例

启动作业并断言执行状态。

AppTest.java

```java
 package com.mkyong;

import static org.junit.Assert.assertEquals;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.batch.core.BatchStatus;
import org.springframework.batch.core.JobExecution;
import org.springframework.batch.test.JobLauncherTestUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {
    "classpath:spring/batch/jobs/spring-batch-job.xml",
    "classpath:spring/batch/config/context.xml",
    "classpath:spring/batch/config/test-context.xml"})
public class AppTest {

    @Autowired
    private JobLauncherTestUtils jobLauncherTestUtils;

    @Test
    public void launchJob() throws Exception {

	//testing a job
        JobExecution jobExecution = jobLauncherTestUtils.launchJob();

	//Testing a individual step
        //JobExecution jobExecution = jobLauncherTestUtils.launchStep("step1");

        assertEquals(BatchStatus.COMPLETED, jobExecution.getStatus());

    }
} 
```

*P.S .假设`context.xml`声明了所有需要的 Spring batch 核心组件，如 jobRepository 等。*

这个`JobLauncherTestUtils`必须手工声明。

test-context.xml

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-3.2.xsd">

    <!-- Spring should auto load this bean -->
    <bean class="org.springframework.batch.test.JobLauncherTestUtils"/>

</beans> 
```

## 4.测试示例

TestNG 框架中的等效示例。

AppTest2.java

```java
 package com.mkyong;

import org.springframework.batch.core.BatchStatus;
import org.springframework.batch.core.JobExecution;
import org.springframework.batch.test.JobLauncherTestUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.testng.AbstractTestNGSpringContextTests;
import org.testng.Assert;
import org.testng.annotations.Test;

@ContextConfiguration(locations = {
    "classpath:spring/batch/jobs/spring-batch-job.xml",
    "classpath:spring/batch/config/context.xml",
    "classpath:spring/batch/config/test-context.xml"})
public class AppTest2 extends AbstractTestNGSpringContextTests {

    @Autowired
    private JobLauncherTestUtils jobLauncherTestUtils;

    @Test
    public void launchJob() throws Exception {

        JobExecution jobExecution = jobLauncherTestUtils.launchJob();
        Assert.assertEquals(jobExecution.getStatus(), BatchStatus.COMPLETED);

    }
} 
```

完成了。

## 下载源代码

Download it – [SpringBatch-UnitTest-Example.zip](http://web.archive.org/web/20190212055412/http://www.mkyong.com/wp-content/uploads/2013/07/SpringBatch-UnitTest-Example.zip) (83 KB)

## 参考

1.  [JobLauncherTestUtils JavaDoc](http://web.archive.org/web/20190212055412/http://static.springsource.org/spring-batch/apidocs/org/springframework/batch/test/JobLauncherTestUtils.html)
2.  [春季批量单元测试官方文档](http://web.archive.org/web/20190212055412/http://static.springsource.org/spring-batch/reference/html/testing.html)

[junit](http://web.archive.org/web/20190212055412/http://www.mkyong.com/tag/junit/) [spring batch](http://web.archive.org/web/20190212055412/http://www.mkyong.com/tag/spring-batch/) [testng](http://web.archive.org/web/20190212055412/http://www.mkyong.com/tag/testng/) [unit test](http://web.archive.org/web/20190212055412/http://www.mkyong.com/tag/unit-test/)







