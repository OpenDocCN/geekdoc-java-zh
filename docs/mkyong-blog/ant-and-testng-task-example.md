# Ant 和 TestNG 任务示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/ant/ant-and-testng-task-example/>

![testng tutorials](img/727dd25e379e20a1f5ee743495fd81d1.png)

在本教程中，我们将向您展示如何在 Ant build 中运行 TestNG 测试。

## 1.按类运行

build.xml

```java
 <taskdef name="testng" classname="org.testng.TestNGAntTask">
	<classpath location="lib/testng-6.8.14.jar" />
  </taskdef>

  <target name="testng" depends="compile">

	<!-- Assume test.path contains the project library dependencies -->
	<testng classpathref="test.path"
		outputDir="${report.dir}" 
		haltOnFailure="true">

		<!-- Extra project classpath, which is not included in above "test.path" -->
		<!-- Tell Ant where is the project and test classes -->
		<classpath location="${test.classes.dir}" />
		<classpath location="${src.classes.dir}" />

		<!-- Tell Ant what test classes need to run -->
		<classfileset dir="${test.classes.dir}" includes="**/*Test*.class" />

	</testng>

  </target> 
```

## 2.由 XML 运行

${resources.dir}/testng.xml

```java
 <!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="TestAll">

    <test name="anyname">
	<classes>
		<class name="com.mkyong.test.TestMessage" />
	</classes>
    </test>

</suite> 
```

build.xml

```java
 <taskdef name="testng" classname="org.testng.TestNGAntTask">
	<classpath location="lib/testng-6.8.14.jar" />
  </taskdef>

  <target name="testng" depends="compile">

	<testng classpathref="test.path"
		outputDir="${report.dir}" 
		haltOnFailure="true">

		<classpath location="${test.classes.dir}" />

		<!-- Tell Ant where is testng.xml -->
		<xmlfileset dir="${resources.dir}" includes="testng.xml"/>

	</testng>

  </target> 
```

## 3.例子

一个 web 应用程序示例，向您展示如何运行一组 TestNG 测试。

3.1 返回消息

MessageGenerator.java

```java
 package com.mkyong.message;

import org.springframework.stereotype.Component;

@Component
public class MessageGenerator {

	public String getWelcomeMessage() {
		return "welcome"; 
	}

} 
```

3.2 两次测试。

TestMessage.java

```java
 package com.mkyong.test;

import org.testng.Assert;
import org.testng.annotations.Test;
import com.mkyong.message.MessageGenerator;

public class TestMessage {

	@Test
	public void test_welcome_message() {
		MessageGenerator obj = new MessageGenerator();
		Assert.assertEquals(obj.getWelcomeMessage(), "welcome");
	}

} 
```

TestMessage2.java

```java
 package com.mkyong.test;

import org.testng.Assert;
import org.testng.annotations.Test;
import com.mkyong.message.MessageGenerator;

public class TestMessage2 {

	@Test
	public void test_welcome_message_2() {
		MessageGenerator obj = new MessageGenerator();
		Assert.assertEquals(obj.getWelcomeMessage(), "welcome");
	}

} 
```

3.3 使用 ivy 获取项目依赖关系，并声明项目范围。

ivy.xml

```java
 <ivy-module version="2.0">
	<info organisation="org.apache" module="WebProject" />

	<configurations>
        <conf name="compile" description="Required to compile application"/>
        <conf name="runtime" description="Additional run-time dependencies" extends="compile"/>
        <conf name="test"    description="Required for test only" extends="runtime"/>
    </configurations>

	<dependencies>
		<dependency org="org.testng" name="testng" rev="6.8.14" conf="test->default" />
	</dependencies>
</ivy-module> 
```

3.4 运行单元测试

build.xml

```java
 <project xmlns:ivy="antlib:org.apache.ivy.ant" 
	name="HelloProject" default="main" basedir=".">
	<description>
		Running TestNG Test 
	</description>

	<!-- Project Structure -->
	<property name="jdk.version" value="1.7" />
	<property name="projectName" value="WebProject" />
	<property name="src.dir" location="src" />
	<property name="test.dir" location="src" />
	<property name="report.dir" location="report" />
	<property name="web.dir" value="war" />
	<property name="web.classes.dir" location="${web.dir}/WEB-INF/classes" />

	<!-- ivy start -->
	<target name="resolve" description="retrieve dependencies with ivy">
		<echo message="Getting dependencies..." />
		<ivy:retrieve />

		<ivy:cachepath pathid="compile.path" conf="compile" />
		<ivy:cachepath pathid="runtime.path" conf="runtime" />
		<ivy:cachepath pathid="test.path" conf="test" />

	</target>

	<!-- Compile Java source from ${src.dir} and output it to ${web.classes.dir} -->
	<target name="compile" depends="init, resolve" description="compile source code">
		<mkdir dir="${web.classes.dir}" />
		<javac destdir="${web.classes.dir}" source="${jdk.version}" 
			target="${jdk.version}" debug="true" includeantruntime="false" classpathref="compile.path">
			<src path="${src.dir}" />
		</javac>
	</target>

	<!-- Run TestNG -->
	<target name="testng" depends="compile">

	  <testng classpathref="test.path"
		outputDir="${report.dir}" 
		haltOnFailure="true">

		<classpath location="${web.classes.dir}" />

		<xmlfileset dir="${resources.dir}" includes="testng.xml"/>

                <!--
		  <classfileset dir="${web.classes.dir}" includes="**/*Test*.class" />
		-->
	  </testng>

	</target>

	<!-- Create folders -->
	<target name="init">
		<mkdir dir="${src.dir}" />
		<mkdir dir="${web.classes.dir}" />
		<mkdir dir="${report.dir}" />
	</target>

	<!-- Delete folders -->
	<target name="clean" description="clean up">
		<delete dir="${web.classes.dir}" />
		<delete dir="${report.dir}" />
	</target>

	<target name="main" depends="testng" />

</project> 
```

${resources.dir}/testng.xml

```java
 <!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="TestAll">

	<test name="example1">
		<classes>
			<class name="com.mkyong.test.TestMessage" />
			<class name="com.mkyong.test.TestMessage2" />
		</classes>
	</test>

</suite> 
```

运行它

```java
 $ ant testng 
```

输出

```java
 testng:
   [testng] [TestNG] Running:
   [testng]   /Users/mkyong/Documents/workspace/AntSpringMVC/resources/testng.xml
   [testng] 
   [testng] 
   [testng] ===============================================
   [testng] TestAll
   [testng] Total tests run: 2, Failures: 0, Skips: 0
   [testng] ===============================================
   [testng] 

BUILD SUCCESSFUL
Total time: 3 seconds 
```

完成了。

## 下载源代码

Download It – [AntSpringMVC-TestNG-Example](http://web.archive.org/web/20221024153708/http://www.mkyong.com/wp-content/uploads/2015/01/AntSpringMVC-TestNG-Example.zip) (90 KB)

## 参考

1.  [Ant–TestNG 任务](http://web.archive.org/web/20221024153708/http://testng.org/doc/ant.html)
2.  [Ang 和 jUnit 任务](http://web.archive.org/web/20221024153708/http://www.mkyong.com/ant/ant-and-junit-task-example/)
3.  [TestNG–套件测试](http://web.archive.org/web/20221024153708/http://www.mkyong.com/unittest/testng-tutorial-5-suite-test/)

<input type="hidden" id="mkyong-current-postId" value="13548">