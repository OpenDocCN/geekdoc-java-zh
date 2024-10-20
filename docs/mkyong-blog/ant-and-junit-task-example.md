# Ant 和 jUnit 任务示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/ant/ant-and-junit-task-example/>

在本教程中，我们将向您展示如何在 Ant build 中运行 junit 测试。

![junit tutorials](img/27a436d334a177edc78426ae02f6b11e.png)

## 1.运行单元测试

build.xml

```java
 <target name="junit" depends="compile">

    <junit printsummary="yes" haltonfailure="no">

	<!-- Project classpath, must include junit.jar -->
	<classpath refid="test.path" />

	<!-- test class -->
	<classpath location="${test.classes.dir}" />

	<test name="com.mkyong.test.TestMessage" 
		haltonfailure="no" todir="${report.dir}">
		<formatter type="plain" />
		<formatter type="xml" />
	</test>

  </junit>
</target> 
```

 ## 2.运行一批单元测试

build.xml

```java
 <target name="junit" depends="compile">

  <junit printsummary="yes" haltonfailure="no">

	<!--
		<classpath location="lib/junit-4.11.jar" />
		<classpath location="lib/hamcrest-core-1.3.jar" />
	-->
	<classpath refid="test.path" />
	<classpath location="${test.classes.dir}" />

	<formatter type="xml" />
	<formatter type="plain" />

	<batchtest fork="yes" todir="${report.dir}">
		<fileset dir="${test.dir}">
			<include name="**/*Test*.java" />
		</fileset>
	</batchtest>

  </junit>
</target> 
```

 ## 3.例子

一个 web 应用程序示例，向您展示如何运行 junit 测试。

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

3.2 测试上述类的两个 junit 测试用例。

TestMessage.java

```java
 package com.mkyong.test;

import static org.junit.Assert.assertEquals;
import org.junit.Test;
import com.mkyong.message.MessageGenerator;

public class TestMessage {

	@Test
	public void test_welcome_message() {
		MessageGenerator obj = new MessageGenerator();
		assertEquals("welcome", obj.getWelcomeMessage());
	}

} 
```

TestMessage2.java

```java
 package com.mkyong.test;

import static org.junit.Assert.assertEquals;
import org.junit.Test;
import com.mkyong.message.MessageGenerator;

public class TestMessage2 {

	@Test
	public void test_welcome_message_2() {
		MessageGenerator obj = new MessageGenerator();
		assertEquals("welcome", obj.getWelcomeMessage());
	}

} 
```

3.3 使用 ivy 获取项目依赖项，并声明项目范围。

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
		<dependency org="junit" name="junit" rev="4.11" conf="test->default" />
	</dependencies>
</ivy-module> 
```

3.4 运行单元测试

build.xml

```java
 <project xmlns:ivy="antlib:org.apache.ivy.ant" 
	name="HelloProject" default="main" basedir=".">
	<description>
		Running junit Test 
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
			target="${jdk.version}" debug="true" 
                        includeantruntime="false" classpathref="compile.path">
			<src path="${src.dir}" />
		</javac>
	</target>

	<!-- Run jUnit -->
	<target name="junit" depends="compile">

	  <junit printsummary="yes" haltonfailure="no">

		<classpath refid="test.path" />
		<classpath location="${web.classes.dir}" />

		<formatter type="xml" />
		<batchtest fork="yes" todir="${report.dir}">
			<fileset dir="${test.dir}">
				<include name="**/*Test*.java" />
			</fileset>
		</batchtest>

	  </junit>
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

	<target name="main" depends="junit" />

</project>
Run it
<pre><code class="language-bash">
$ ant junit 
```

完成了。

## 下载源代码

Download It - [AntSpringMVC-Junit-Example](http://web.archive.org/web/20190310093416/http://www.mkyong.com/wp-content/uploads/2015/01/AntSpringMVC-Junit-Example.zip) (42 KB)

## 参考

1.  [Ant - junit 任务](http://web.archive.org/web/20190310093416/http://ant.apache.org/manual/Tasks/junit.html)

[ant](http://web.archive.org/web/20190310093416/http://www.mkyong.com/tag/ant/) [junit](http://web.archive.org/web/20190310093416/http://www.mkyong.com/tag/junit/) [unit test](http://web.archive.org/web/20190310093416/http://www.mkyong.com/tag/unit-test/)







