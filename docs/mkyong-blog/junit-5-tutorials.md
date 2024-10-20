# JUnit 5 教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-tutorials/>

![junit 5 logo](img/46092775dbf2f607928c9c130c58a378.png)

```java
 JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage 
```

*P.S JUnit 5 在运行时需要 Java 8(或更高版本)*

## 1.6 月 5 日+胃

参见这个完整的 [JUnit 5 + Maven](/web/20221230032500/https://mkyong.com/junit5/junit-5-maven-examples/) 示例。

pom.xml

```java
 <dependency>
	<groupId>org.junit.jupiter</groupId>
	<artifactId>junit-jupiter-engine</artifactId>
	<version>5.5.2</version>
	<scope>test</scope>
</dependency>

<build>
    <plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-surefire-plugin</artifactId>
			<version>3.0.0-M3</version>
		</plugin>
	</plugins>
</build> 
```

*附言`maven-surefire-plugin`必须至少是版本`2.22.0`*

## 2.JUnit 5 + Gradle

查看完整的 JUnit 5 + Gradle 示例。

gradle.build

```java
 plugins {
	id 'java'
	id 'eclipse' // optional, for Eclipse project
	id 'idea'	 // optional, for IntelliJ IDEA project
}

repositories {
	mavenCentral()
}

dependencies {
	testImplementation('org.junit.jupiter:junit-jupiter:5.5.2')
}

test {
	useJUnitPlatform()
} 
```

## 3.JUnit 5 测试

*   [六月五日@DisplayName](/web/20221230032500/https://mkyong.com/junit5/junit-5-display-names/)
*   [JUnit 5 假设](/web/20221230032500/https://mkyong.com/junit5/junit-5-assumptions-examples/)
*   [JUnit 5 @禁用](/web/20221230032500/https://mkyong.com/junit5/junit-5-how-to-disable-tests/)
*   [JUnit 5 条件测试示例](/web/20221230032500/https://mkyong.com/junit5/junit-5-conditional-test-examples/)
*   [JUnit 5 标签和过滤，@Tag](/web/20221230032500/https://mkyong.com/junit5/junit-5-tagging-and-filtering-tag-examples/)
*   [JUnit 5 测试执行顺序，@TestMethodOrder](/web/20221230032500/https://mkyong.com/junit5/junit-5-test-execution-order/)
*   [JUnit 5 嵌套测试](/web/20221230032500/https://mkyong.com/junit5/junit-5-nested-test-examples/)
*   [JUnit 5 重复测试](/web/20221230032500/https://mkyong.com/junit5/junit-5-repeated-tests/)
*   [JUnit 5 从控制台运行测试](/web/20221230032500/https://mkyong.com/junit5/junit-5-consolelauncher-examples/)
*   [JUnit 5 参数化测试](/web/20221230032500/https://mkyong.com/junit5/junit-5-parameterized-tests/)
*   [JUnit 5 超时](/web/20221230032500/https://mkyong.com/junit5/junit-5-timeouts-examples/)
*   [JUnit 5 预期异常](/web/20221230032500/https://mkyong.com/junit5/junit-5-expected-exception/)
*   JUnit 5 断言

## 4.第三方断言库

*   [JUnit 5 + AssertJ](/web/20221230032500/https://mkyong.com/junit5/junit-5-assertj-examples/)
*   JUnit 5 + Hamcrest
*   JUnit 5 +真相

## 5.综合

*   JUnit 5 + JUnit 4 一起
*   JUnit 5 + Mockito
*   [朱尼特 5 + Spring Boot](/web/20221230032500/https://mkyong.com/spring-boot/spring-boot-junit-5-mockito/)
*   JUnit 5+https://cucumber.io/

## 下载源代码

$ git 克隆[https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221230032500/https://github.com/mkyong/junit-examples)

## 参考

*   [JUnit 5 官网](http://web.archive.org/web/20221230032500/https://junit.org/junit5/)
*   [JUnit 5 用户指南](http://web.archive.org/web/20221230032500/https://junit.org/junit5/docs/current/user-guide/)
*   [JUnit 5 样本库](http://web.archive.org/web/20221230032500/https://github.com/junit-team/junit5-samples)
*   JUnit 5 指南
*   Java 测试& JVM 项目

<input type="hidden" id="mkyong-current-postId" value="15790">