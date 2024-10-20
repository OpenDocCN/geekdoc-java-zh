# 使用 CommandLineJobRunner 运行春季批处理作业

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-batch/run-spring-batch-job-with-commandlinejobrunner/>

向您展示如何使用`CommandLineJobRunner`运行 Spring 批处理作业的快速指南。

## 1.春季批处理作业示例

一份简单的工作。

resources/spring/batch/jobs/job-read-files.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<beans ...
   <import resource="../config/context.xml"/>

   <job id="readJob" >
      <step id="step1">
	<tasklet>
		<chunk reader="flatFileItemReader" 
                          writer="flatFileItemWriter" commit-interval="1" />
	</tasklet>
      </step>
   </job>

	<!-- ... -->
</beans> 
```

 ## 2.包项目

使用 Maven 将您的项目打包成一个 jar 文件-**target/your-project . jar**，并将所有依赖项复制到 **target/dependency-jars/** 。

pom.xml

```java
 <!-- ... -->
  <plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-dependency-plugin</artifactId>
	<version>2.5.1</version>
	<executions>
	  <execution>
		<id>copy-dependencies</id>
		<phase>package</phase>
		<goals>
			<goal>copy-dependencies</goal>
		</goals>
		<configuration>
			<outputDirectory>
				${project.build.directory}/dependency-jars/
			</outputDirectory>
		</configuration>
	  </execution>
	</executions>
  </plugin> 
```

```java
 $ mvn package 
```

 ## 3.CommandLineJobRunner 示例

用法:

```java
 CommandLineJobRunner jobPath <options> jobIdentifier (jobParameters) 
```

要运行以上 spring 批处理作业，请键入以下命令:

```java
 $ java -cp "target/dependency-jars/*:target/your-project.jar" org.springframework.batch.core.launch.support.CommandLineJobRunner spring/batch/jobs/job-read-files.xml readJob 
```

对于`jobParameters`，附加到命令的末尾:

```java
 $ java -cp "target/dependency-jars/*:target/your-project.jar" org.springframework.batch.core.launch.support.CommandLineJobRunner spring/batch/jobs/job-read-files.xml readJob file.name=testing.cvs 
```

要按计划运行它，通常可以将上述命令复制到一个`.sh`文件中，并使用任何调度程序命令运行它，比如*nix 中的`cron`。参考这个例子——[在 Linux 下给 cron 添加作业](http://web.archive.org/web/20190223082639/http://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)。

当批处理作业在系统调度程序下运行时，确保它可以找到你的项目的类路径。

## 下载源代码

Download it – [SpringBatch-Run-Example.zip](http://web.archive.org/web/20190223082639/http://www.mkyong.com/wp-content/uploads/2013/07/SpringBatch-Run-Example.zip)(12 KB)

## 参考

1.  [CommandLineJobRunner JavaDoc](http://web.archive.org/web/20190223082639/http://static.springsource.org/spring-batch/apidocs/org/springframework/batch/core/launch/support/CommandLineJobRunner.html)
2.  [如何用 Maven 创建 Jar 文件](http://web.archive.org/web/20190223082639/http://www.mkyong.com/maven/how-to-create-a-jar-file-with-maven/)
3.  [春批 Hello World——记忆中的](http://web.archive.org/web/20190223082639/http://www.techavalanche.com/2011/08/21/spring-batch-hello-world-in-memory/)

[spring batch](http://web.archive.org/web/20190223082639/http://www.mkyong.com/tag/spring-batch/)







