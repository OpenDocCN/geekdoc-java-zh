# JUnit + Spring 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-spring-integration-example/>

在本教程中，我们将向您展示如何使用 JUnit 框架测试 Spring DI 组件。

使用的技术:

1.  JUnit 4.12
2.  哈姆克雷斯特 1.3
3.  弹簧 4.3.0 .释放
4.  专家

## 1.项目相关性

要集成 Spring 和 JUnit，您需要`spring-test.jar`

pom.xml

```java
 <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.hamcrest</groupId>
                    <artifactId>hamcrest-core</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-library</artifactId>
            <version>1.3</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>4.3.0.RELEASE</version>
            <scope>test</scope>
        </dependency> 
```

## 2.弹簧组件

一个简单的弹簧组件，稍后进行测试。

2.1 一个接口。

DataModelService.java

```java
 package com.mkyong.examples.spring;

public interface DataModelService {

    boolean isValid(String input);

} 
```

2.2 上述接口的实现。

MachineLearningService.java

```java
 package com.mkyong.examples.spring;

import org.springframework.stereotype.Service;

@Service("ml")
public class MachineLearningService implements DataModelService {

    @Override
    public boolean isValid(String input) {
        return true;
    }

} 
```

2.2 一个 Spring 配置文件，组件扫描。

AppConfig.java

```java
 package com.mkyong.examples.spring;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = {"com.mkyong.examples.spring"})
public class AppConfig {
} 
```

## 3.JUnit + Spring 集成示例

用`@RunWith(SpringJUnit4ClassRunner.class)`注释 JUnit 测试类，并手动加载 Spring 配置文件。参考下文:

MachineLearningTest.java

```java
 package com.mkyong.spring;

import com.mkyong.examples.spring.AppConfig;
import com.mkyong.examples.spring.DataModelService;
import com.mkyong.examples.spring.MachineLearningService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.instanceOf;
import static org.hamcrest.Matchers.is;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {AppConfig.class})
public class MachineLearningTest {

	//DI
    @Autowired
    @Qualifier("ml")
    DataModelService ml;

    @Test
    public void test_ml_always_return_true() {

        //assert correct type/impl
        assertThat(ml, instanceOf(MachineLearningService.class));

        //assert true
        assertThat(ml.isValid(""), is(true));

    }
} 
```

完成了。

## 4.常见问题

4.1 对于 XML，请尝试:

```java
 import org.springframework.test.context.ContextConfiguration;

@ContextConfiguration(locations = {
        "classpath:pathTo/appConfig.xml",
        "classpath:pathTo/appConfig2.xml"})
public class MachineLearningTest {
//...
} 
```

4.2 对于多个配置文件:

```java
 import org.springframework.test.context.ContextConfiguration;

@ContextConfiguration(classes = {AppConfig.class, AppConfig2.class})
public class MachineLearningTest {
//...
} 
```

## 参考

1.  [Spring IO–单元测试](http://web.archive.org/web/20201111193131/http://docs.spring.io/spring-batch/reference/html/testing.html)
2.  [Spring IO–集成测试](http://web.archive.org/web/20201111193131/http://docs.spring.io/spring/docs/current/spring-framework-reference/html/integration-testing.html)
3.  [TestNG + Spring 集成示例](http://web.archive.org/web/20201111193131/http://www.mkyong.com/unittest/testng-spring-integration-example/)
4.  [春季批量单元测试示例](http://web.archive.org/web/20201111193131/http://www.mkyong.com/spring-batch/spring-batch-unit-test-example/)

Tags : [junit](http://web.archive.org/web/20201111193131/https://mkyong.com/tag/junit/) [spring](http://web.archive.org/web/20201111193131/https://mkyong.com/tag/spring/)<input type="hidden" id="mkyong-current-postId" value="14009">

### 相关文章

*   [如果 junit.jar 不在 Ant 自己的 classpa 中，则必须包含它](/web/20201111193131/https://mkyong.com/ant/ant-error-must-include-junit-jar-if-not-in-ants-own-classpath/)
*   [Ant 和 jUnit 任务示例](/web/20201111193131/https://mkyong.com/ant/ant-and-junit-task-example/)
*   [如何用 Maven 运行单元测试](/web/20201111193131/https://mkyong.com/maven/how-to-run-unit-test-with-maven/)
*   [Wicket + Spring 集成示例](/web/20201111193131/https://mkyong.com/wicket/wicket-spring-integration-example/)
*   [Spring AOP 拦截器事务不工作](/web/20201111193131/https://mkyong.com/spring/spring-aop-interceptor-transaction-is-not-working/)

*   [Spring Autowiring @Qualifier 示例](/web/20201111193131/https://mkyong.com/spring/spring-autowiring-qualifier-example/)
*   [Maven + Spring hello world 示例](/web/20201111193131/https://mkyong.com/spring/quick-start-maven-spring-example/)
*   [如何加载多个弹簧豆配置文件](/web/20201111193131/https://mkyong.com/spring/load-multiple-spring-bean-configuration-file/)
*   [弹簧松耦合示例](/web/20201111193131/https://mkyong.com/spring/spring-loosely-coupled-example/)
*   [如何在 Eclipse 中安装 Spring IDE](/web/20201111193131/https://mkyong.com/spring/how-to-install-spring-ide-in-eclipse/)