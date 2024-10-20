# JUnit 5 + Gradle 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/gradle/junit-5-gradle-examples/>

![junit 5 logo](img/d9ed889084139b5e5f6d1a0b435d495b.png)

本文向您展示了如何在 Gradle 项目中添加 JUnit 5。

使用的技术:

*   Gradle 5.4.1
*   Java 8
*   JUnit 5.5.2

## 1.Gradle + JUnit 5

1.添加 JUni 5 jupiter 引擎，并如下定义`useJUnitPlatform()`:

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

## 2.Gradle 项目

标准的 Java 项目结构。

![project structure](img/0b644bc219376c03380957a440e05531.png)

## 3.JUnit 5

3.1 一个简单的单元测试例子。

MessageService.java

```java
 package com.mkyong.core;

public class MessageService {

    public static String get() {
        return "Hello JUnit 5";
    }

} 
```

3.2 JUnit 5 简单`Assertions`测试。

MessageServiceTest.java

```java
 package com.mkyong.core;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class MessageServiceTest {

    @DisplayName("Test MessageService.get()")
    @Test
    void testGet() {
        assertEquals("Hello JUnit 5", MessageService.get());
    }

} 
```

## 4 .测试等级

4.1 在 Gradle 中运行测试。

Terminal

```java
 $ cd project 
$ gradle test 

BUILD SUCCESSFUL in 0s
3 actionable tasks: 3 up-to-date 
```

4.2 如果测试失败，它将显示如下内容:

Terminal

```java
 $ gradle test 

> Task :test FAILED

com.mkyong.core.MessageServiceTest > testGet() FAILED
    org.opentest4j.AssertionFailedError at MessageServiceTest.java:13

1 test completed, 1 failed 
```

4.3 `gradle test`默认生成如下 HTML 测试报告:

`{project}\build\reports\tests\test\index.html`![test report](img/a1286c22eaf16e4c88909ba5d54c16ab.png)

完成了。

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221225035521/https://github.com/mkyong/junit-examples)
$ cd junit5-gradle
$ gradle test

# 参考

*   [JUnit 5 官方网站](http://web.archive.org/web/20221225035521/https://junit.org/junit5/)
*   [Spring Boot +朱尼特 5 +莫奇托](http://web.archive.org/web/20221225035521/https://www.mkyong.com/spring-boot/spring-boot-junit-5-mockito/)
*   Java 测试& JVM 项目

<input type="hidden" id="mkyong-current-postId" value="15181">