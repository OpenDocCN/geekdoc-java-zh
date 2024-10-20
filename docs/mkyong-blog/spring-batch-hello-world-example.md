# 春季批次 Hello World 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-batch/spring-batch-hello-world-example/>

Spring Batch 是一个批处理框架——执行一系列作业。在 Spring Batch 中，一个作业由许多步骤组成，每个步骤由一个`READ-PROCESS-WRITE`任务或`single operation` 任务(小任务)组成。

1.  对于“读取-处理-写入”过程，它意味着从资源(csv、xml 或数据库)中“读取”数据，“处理”它并将它“写入”到其他资源(csv、xml 和数据库)。例如，一个步骤可以从 CSV 文件中读取数据，对其进行处理并将其写入数据库。Spring Batch 提供了许多用来读/写 CSV、XML 和数据库的类。
2.  对于“单个”操作任务(tasklet)，它意味着只执行单个任务，比如在一个步骤开始或完成之前或之后清理资源。
3.  并且这些步骤可以链接在一起作为一个作业运行。

```java
 1 Job = Many Steps.
1 Step = 1 READ-PROCESS-WRITE or 1 Tasklet.
Job = {Step 1 -> Step 2 -> Step 3} (Chained together) 
```

## 春季批次示例

考虑以下批处理作业:

1.  步骤 1–从文件夹 A 读取 CSV 文件，处理，将其写入文件夹 b。“读取-处理-写入”
2.  步骤 2-从文件夹 B 中读取 CSV 文件，处理，将其写入数据库。"读取-处理-写入"
3.  步骤 3–从文件夹 b“Tasklet”中删除 CSB 文件
4.  步骤 4-从数据库中读取数据，处理并生成 XML 格式的统计报告，将其写入文件夹 c。“读取-处理-写入”
5.  第 5 步-阅读报告并将其发送至经理邮箱。“小任务”

在 Spring 批处理中，我们可以声明如下:

```java
 <job id="abcJob" >
	<step id="step1" next="step2">
	  <tasklet>
		<chunk reader="cvsItemReader" writer="cvsItemWriter"  
                    processor="itemProcesser" commit-interval="1" />
	  </tasklet>
	</step>
	<step id="step2" next="step3">
	  <tasklet>
		<chunk reader="cvsItemReader" writer="databaseItemWriter"  
                    processor="itemProcesser" commit-interval="1" />
	  </tasklet>
	</step>
	<step id="step3" next="step4">
	  <tasklet ref="fileDeletingTasklet" />
	</step>
	<step id="step4" next="step5">
	  <tasklet>
		<chunk reader="databaseItemReader" writer="xmlItemWriter"  
                    processor="itemProcesser" commit-interval="1" />
	  </tasklet>
	</step>
	<step id="step5">
		<tasklet ref="sendingEmailTasklet" />
	</step>
  </job> 
```

整个作业和步骤的执行都存储在数据库中，这使得失败的步骤能够从失败的地方重新开始，而不需要重新开始整个作业。

 ## 1.辅导的

在这个 Spring 批处理教程中，我们将向您展示如何创建作业、读取 CSV 文件、处理它、将输出写入 XML 文件。

使用的工具和库

1.  maven3
2.  Eclipse 4.2
3.  JDK 1.6
4.  弹簧芯 3.2.2 .释放
5.  春季 OXM 3.2.2 .发布
6.  春季 JDBC 3.2.2 .发布
7.  春季批次 2.2.0 .发布

 ## 2.项目目录

查看最终项目目录，这是一个标准的 Maven 项目。

![spring-batch-hello-world-directory](img/c7e69c584dc777f52d5bb3a474a36aa2.png)

## 3.项目相关性

他们必须依赖的只是 Spring Core、Spring Batch 和 JDK 1.5。阅读注释，了解更多信息。

pom.xml

```java
 <project  
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
	http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.mkyong</groupId>
	<artifactId>SpringBatchExample</artifactId>
	<packaging>jar</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>SpringBatchExample</name>
	<url>http://maven.apache.org</url>

	<properties>
		<jdk.version>1.6</jdk.version>
		<spring.version>3.2.2.RELEASE</spring.version>
		<spring.batch.version>2.2.0.RELEASE</spring.batch.version>
		<mysql.driver.version>5.1.25</mysql.driver.version>
		<junit.version>4.11</junit.version>
	</properties>

	<dependencies>

		<!-- Spring Core -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- Spring jdbc, for database -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- Spring XML to/back object -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-oxm</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- MySQL database driver -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${mysql.driver.version}</version>
		</dependency>

		<!-- Spring Batch dependencies -->
		<dependency>
			<groupId>org.springframework.batch</groupId>
			<artifactId>spring-batch-core</artifactId>
			<version>${spring.batch.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.batch</groupId>
			<artifactId>spring-batch-infrastructure</artifactId>
			<version>${spring.batch.version}</version>
		</dependency>

		<!-- Spring Batch unit test -->
		<dependency>
			<groupId>org.springframework.batch</groupId>
			<artifactId>spring-batch-test</artifactId>
			<version>${spring.batch.version}</version>
		</dependency>

		<!-- Junit -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>${junit.version}</version>
			<scope>test</scope>
		</dependency>

	</dependencies>
	<build>
		<finalName>spring-batch</finalName>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-eclipse-plugin</artifactId>
				<version>2.9</version>
				<configuration>
					<downloadSources>true</downloadSources>
					<downloadJavadocs>false</downloadJavadocs>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.3.2</version>
				<configuration>
					<source>${jdk.version}</source>
					<target>${jdk.version}</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project> 
```

## 4.春季批处理作业

CSV 文件。

report.csv

```java
 1001,"213,100",980,"mkyong", 29/7/2013
1002,"320,200",1080,"staff 1", 30/7/2013
1003,"342,197",1200,"staff 2", 31/7/2013 
```

一个 Spring 批处理作业，用`FlatFileItemReader`读取上述 csv 文件，用`itemProcessor`处理数据，用`StaxEventItemWriter`将数据写入 XML 文件
。

job-hello-world.xml

```java
 <beans 
	xmlns:batch="http://www.springframework.org/schema/batch" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/batch
		http://www.springframework.org/schema/batch/spring-batch-2.2.xsd
		http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
	">

	<import resource="../config/context.xml" />
	<import resource="../config/database.xml" />

	<bean id="report" class="com.mkyong.model.Report" scope="prototype" />
	<bean id="itemProcessor" class="com.mkyong.CustomItemProcessor" />

	<batch:job id="helloWorldJob">
	  <batch:step id="step1">
		<batch:tasklet>
			<batch:chunk reader="cvsFileItemReader" writer="xmlItemWriter" 
                              processor="itemProcessor" commit-interval="10">
			</batch:chunk>
		</batch:tasklet>
	  </batch:step>
	</batch:job>

	<bean id="cvsFileItemReader" class="org.springframework.batch.item.file.FlatFileItemReader">

		<property name="resource" value="classpath:cvs/input/report.csv" />

		<property name="lineMapper">
		    <bean class="org.springframework.batch.item.file.mapping.DefaultLineMapper">
			<property name="lineTokenizer">
				<bean
					class="org.springframework.batch.item.file.transform.DelimitedLineTokenizer">
					<property name="names" value="id,sales,qty,staffName,date" />
				</bean>
			</property>
			<property name="fieldSetMapper">
				<bean class="com.mkyong.ReportFieldSetMapper" />

				 <!-- if no data type conversion, use BeanWrapperFieldSetMapper to map by name
				<bean
					class="org.springframework.batch.item.file.mapping.BeanWrapperFieldSetMapper">
					<property name="prototypeBeanName" value="report" />
				</bean>
				 -->
			</property>
		    </bean>
		</property>

	</bean>

	<bean id="xmlItemWriter" class="org.springframework.batch.item.xml.StaxEventItemWriter">
		<property name="resource" value="file:xml/outputs/report.xml" />
		<property name="marshaller" ref="reportMarshaller" />
		<property name="rootTagName" value="report" />
	</bean>

	<bean id="reportMarshaller" class="org.springframework.oxm.jaxb.Jaxb2Marshaller">
	   <property name="classesToBeBound">
		<list>
			<value>com.mkyong.model.Report</value>
		</list>
	    </property>
	</bean>

</beans> 
```

将 CSV 值映射到`Report`对象，并将其写入 XML 文件(通过 jaxb 注释)。

Report.java

```java
 package com.mkyong.model;

import java.math.BigDecimal;
import java.util.Date;
import javax.xml.bind.annotation.XmlAttribute;
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name = "record")
public class Report {

	private int id;
	private BigDecimal sales;
	private int qty;
	private String staffName;
	private Date date;

	@XmlAttribute(name = "id")
	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	@XmlElement(name = "sales")
	public BigDecimal getSales() {
		return sales;
	}

	public void setSales(BigDecimal sales) {
		this.sales = sales;
	}

	@XmlElement(name = "qty")
	public int getQty() {
		return qty;
	}

	public void setQty(int qty) {
		this.qty = qty;
	}

	@XmlElement(name = "staffName")
	public String getStaffName() {
		return staffName;
	}

	public void setStaffName(String staffName) {
		this.staffName = staffName;
	}

	public Date getDate() {
		return date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

	@Override
	public String toString() {
		return "Report [id=" + id + ", sales=" + sales 
                    + ", qty=" + qty + ", staffName=" + staffName + "]";
	}

} 
```

要转换一个`Date`，需要一个自定义的`FieldSetMapper`。如果没有数据类型转换，只需使用`BeanWrapperFieldSetMapper`自动按名称映射值。

ReportFieldSetMapper.java

```java
 package com.mkyong;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import org.springframework.batch.item.file.mapping.FieldSetMapper;
import org.springframework.batch.item.file.transform.FieldSet;
import org.springframework.validation.BindException;

import com.mkyong.model.Report;

public class ReportFieldSetMapper implements FieldSetMapper<Report> {

	private SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");

	@Override
	public Report mapFieldSet(FieldSet fieldSet) throws BindException {

		Report report = new Report();
		report.setId(fieldSet.readInt(0));
		report.setSales(fieldSet.readBigDecimal(1));
		report.setQty(fieldSet.readInt(2));
		report.setStaffName(fieldSet.readString(3));

		//default format yyyy-MM-dd
		//fieldSet.readDate(4);
		String date = fieldSet.readString(4);
		try {
			report.setDate(dateFormat.parse(date));
		} catch (ParseException e) {
			e.printStackTrace();
		}

		return report;

	}

} 
```

itemProcessor 将在 itemWriter 之前被激发。

CustomItemProcessor.java

```java
 package com.mkyong;

import org.springframework.batch.item.ItemProcessor;
import com.mkyong.model.Report;

public class CustomItemProcessor implements ItemProcessor<Report, Report> {

	@Override
	public Report process(Report item) throws Exception {

		System.out.println("Processing..." + item);
		return item;
	}

} 
```

Spring 上下文和数据库配置。

context.xml

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd">

	<!-- stored job-meta in memory -->
	<!--  
	<bean id="jobRepository"
		class="org.springframework.batch.core.repository.support.MapJobRepositoryFactoryBean">
		<property name="transactionManager" ref="transactionManager" />
	</bean>
 	 -->

 	 <!-- stored job-meta in database -->
	<bean id="jobRepository"
		class="org.springframework.batch.core.repository.support.JobRepositoryFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="transactionManager" ref="transactionManager" />
		<property name="databaseType" value="mysql" />
	</bean>

	<bean id="transactionManager"
		class="org.springframework.batch.support.transaction.ResourcelessTransactionManager" />

	<bean id="jobLauncher"
		class="org.springframework.batch.core.launch.support.SimpleJobLauncher">
		<property name="jobRepository" ref="jobRepository" />
	</bean>

</beans> 
```

database.xml

```java
 <beans 
	xmlns:jdbc="http://www.springframework.org/schema/jdbc" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
		http://www.springframework.org/schema/jdbc 
		http://www.springframework.org/schema/jdbc/spring-jdbc-3.2.xsd">

        <!-- connect to MySQL database -->
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/test" />
		<property name="username" value="root" />
		<property name="password" value="" />
	</bean>

	<bean id="transactionManager"
		class="org.springframework.batch.support.transaction.ResourcelessTransactionManager" />

	<!-- create job-meta tables automatically -->
	<jdbc:initialize-database data-source="dataSource">
		<jdbc:script location="org/springframework/batch/core/schema-drop-mysql.sql" />
		<jdbc:script location="org/springframework/batch/core/schema-mysql.sql" />
	</jdbc:initialize-database>

</beans> 
```

#### 5.运行它

运行批处理作业的最简单方法。

```java
 package com.mkyong;

import org.springframework.batch.core.Job;
import org.springframework.batch.core.JobExecution;
import org.springframework.batch.core.JobParameters;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
  public static void main(String[] args) {

	String[] springConfig  = 
		{	
			"spring/batch/jobs/job-hello-world.xml" 
		};

	ApplicationContext context = 
			new ClassPathXmlApplicationContext(springConfig);

	JobLauncher jobLauncher = (JobLauncher) context.getBean("jobLauncher");
	Job job = (Job) context.getBean("helloWorldJob");

	try {

		JobExecution execution = jobLauncher.run(job, new JobParameters());
		System.out.println("Exit Status : " + execution.getStatus());

	} catch (Exception e) {
		e.printStackTrace();
	}

	System.out.println("Done");

  }
} 
```

输出

report.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<report>
	<record id="1001">
		<date>2013-07-29T00:00:00+08:00</date>
		<qty>980</qty>
		<sales>213100</sales>
		<staffName>mkyong</staffName>
	</record>
	<record id="1002">
		<date>2013-07-30T00:00:00+08:00</date>
		<qty>1080</qty>
		<sales>320200</sales>
		<staffName>staff 1</staffName>
	</record>
	<record id="1003">
		<date>2013-07-31T00:00:00+08:00</date>
		<qty>1200</qty>
		<sales>342197</sales>
		<staffName>staff 2</staffName>
	</record>
</report> 
```

在控制台中

```java
 Jul 30, 2013 11:52:00 PM org.springframework.batch.core.launch.support.SimpleJobLauncher$1 run
INFO: Job: [FlowJob: [name=helloWorldJob]] launched with the following parameters: [{}]
Jul 30, 2013 11:52:00 PM org.springframework.batch.core.job.SimpleStepHandler handleStep
INFO: Executing step: [step1]
Processing...Report [id=1001, sales=213100, qty=980, staffName=mkyong]
Processing...Report [id=1002, sales=320200, qty=1080, staffName=staff 1]
Processing...Report [id=1003, sales=342197, qty=1200, staffName=staff 2]
Jul 30, 2013 11:52:00 PM org.springframework.batch.core.launch.support.SimpleJobLauncher$1 run
INFO: Job: [FlowJob: [name=helloWorldJob]] completed with the following parameters: [{}] and the following status: [COMPLETED]
Exit Status : COMPLETED
Done 
```

## 下载源代码

Download it – [SpringBatch-Hello-World-Example.zip](http://web.archive.org/web/20190304005017/http://www.mkyong.com/wp-content/uploads/2013/07/SpringBatch-Hello-World-Example.zip) (27 kb)

## 参考

1.  [什么是批处理](http://web.archive.org/web/20190304005017/http://en.wikipedia.org/wiki/Batch_processing)
2.  [春季批量框架](http://web.archive.org/web/20190304005017/http://static.springsource.org/spring-batch/)
3.  [春季批次–参考文件](http://web.archive.org/web/20190304005017/http://static.springsource.org/spring-batch/reference/html/)

[hello world](http://web.archive.org/web/20190304005017/http://www.mkyong.com/tag/hello-world/) [spring batch](http://web.archive.org/web/20190304005017/http://www.mkyong.com/tag/spring-batch/)







