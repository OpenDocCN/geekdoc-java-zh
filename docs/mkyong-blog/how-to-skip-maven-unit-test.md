# maven——如何跳过单元测试

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/how-to-skip-maven-unit-test/>

在 Maven 中，您可以定义一个系统属性`-Dmaven.test.skip=true`来跳过整个单元测试。

默认情况下，在构建项目时，Maven 会自动运行整个单元测试。如果任何单元测试失败，它将迫使 Maven 中止构建过程。在现实生活中，即使有些案例失败了，你可能仍然需要构建你的项目。

在本文中，我们将向您展示几种跳过单元测试的方法。

## 1.maven.test.skip=true

1.1 为了跳过单元测试，使用了这个参数`-Dmaven.test.skip=true`

Terminal

```java
 $ mvn package -Dmaven.test.skip=true
#no test 
```

1.2 或在`pom.xml`中定义

pom.xml

```java
 <properties>
        <maven.test.skip>true</maven.test.skip>
    </properties> 
```

Terminal

```java
 $ mvn package
#no test 
```

## 2.Maven Surefire 插件

2.1 或者，在 surefire 插件中使用这个`-DskipTests`。

Terminal

```java
 $ mvn package -DskipTests
#no test 
```

2.2 或者这个。

pom.xml

```java
 <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0-M1</version>
    <configuration>
        <skipTests>true</skipTests>
    </configuration>
</plugin> 
```

2.3 跳过一些测试类。

pom.xml

```java
 <plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-surefire-plugin</artifactId>
		<version>3.0.0-M1</version>
		<configuration>
			<excludes>
				<exclude>**/TestMagic*.java</exclude>
				<exclude>**/TestMessage*.java</exclude>
			</excludes>
		</configuration>
    </plugin> 
```

## 3.Maven 简介

3.1 创建一个自定义概要文件来跳过单元测试。

pom.xml

```java
 <profiles>
        <profile>
			<id>xtest</id>
			<properties>
				<maven.test.skip>true</maven.test.skip>
			</properties>
		</profile>
    </profiles> 
```

3.2 使用`-P`选项激活轮廓。

Terminal

```java
 $ mvn package -Pxtest
#no test 
```

## 参考

1.  [Maven Surefire 插件](http://web.archive.org/web/20221212193805/https://maven.apache.org/surefire/maven-surefire-plugin/index.html)
2.  [试验的包含和排除](http://web.archive.org/web/20221212193805/https://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html)
3.  [跳过测试](http://web.archive.org/web/20221212193805/https://maven.apache.org/surefire/maven-surefire-plugin/examples/skipping-tests.html)
4.  [Maven 概要示例](http://web.archive.org/web/20221212193805/http://www.mkyong.com/maven/maven-profiles-example/)
5.  [如何用 Maven 运行单元测试](http://web.archive.org/web/20221212193805/http://www.mkyong.com/maven/how-to-run-unit-test-with-maven/)

<input type="hidden" id="mkyong-current-postId" value="1258">