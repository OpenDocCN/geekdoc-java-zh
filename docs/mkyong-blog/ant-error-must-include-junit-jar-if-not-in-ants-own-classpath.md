# 如果不在 Ant 自己的类路径中，则必须包含 junit.jar

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/ant/ant-error-must-include-junit-jar-if-not-in-ants-own-classpath/>

在 Ant 中声明一个 junit 任务，如下所示

build.xml

```java
 <!-- Run jUnit -->
  <target name="junit" depends="resolve">

	<junit printsummary="yes" haltonfailure="no">

		<classpath refid="test.path" />
		<classpath location="${build.dir}" />

		<test name="com.mkyong.test.TestMessage" 
			haltonfailure="no" todir="${report.dir}" outfile="result">
			<formatter type="plain" />
			<formatter type="xml" />
		</test>

	</junit>
  </target> 
```

运行`ant junit`，但出现以下错误信息:

```java
 BUILD FAILED
build.xml:86: The <classpath> for <junit> must include junit.jar if not in Ant's own classpath 
```

## 解决办法

要在 Ant 中运行 junit 任务，请确保在类路径中定义了`junit.jar`。

build.xml

```java
 <!-- Run jUnit -->
  <target name="junit" depends="resolve">

	<junit printsummary="yes" haltonfailure="no">

		<classpath refid="test.path" />
		<classpath location="${build.dir}" />

		<!-- Make sure these two libraries are included -->
		<classpath location="lib/junit-4.11.jar" />
		<classpath location="lib/hamcrest-core-1.3.jar" />

		<test name="com.mkyong.test.TestMessage" 
			haltonfailure="no" todir="${report.dir}" outfile="result">
			<formatter type="plain" />
			<formatter type="xml" />
		</test>

	</junit>
  </target> 
```

## 参考

1.  [蚂蚁 jUnit 任务](http://web.archive.org/web/20221024153638/https://ant.apache.org/manual/Tasks/junit.html)
2.  Ant 常见问题:junit 忽略了我的类路径

<input type="hidden" id="mkyong-current-postId" value="13544">