# NoSuchBeanDefinitionException:没有 JobLauncherTestUtils 类型的合格 bean

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-batch/nosuchbeandefinitionexception-no-qualifying-bean-of-type-joblaunchertestutils/>

遵循[官方 Spring 批量单元测试指南](http://web.archive.org/web/20190223065023/http://static.springsource.org/spring-batch/reference/html/testing.html)创建一个标准的单元测试用例。

```java
 @RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {
    "classpath:spring/batch/jobs/job-abc.xml",
    "classpath:spring/batch/config/context.xml"})

public class AppTest {

    @Autowired
    private JobLauncherTestUtils jobLauncherTestUtils;

    @Test
    public void launchJob() throws Exception {

        JobExecution jobExecution = jobLauncherTestUtils.launchJob();
        assertEquals(BatchStatus.COMPLETED, jobExecution.getStatus());

    }
} 
```

*P.S `spring-batch-test.jar`被添加到类路径中。*

## 问题

启动上述单元测试时，提示`JobLauncherTestUtils`没有这样的 bean 错误信息？

```java
 org.springframework.beans.factory.BeanCreationException: Could not autowire field: 
	private org.springframework.batch.test.JobLauncherTestUtils com.mkyong.AppTest.jobLauncherTestUtils; 
	......
org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 
	[org.springframework.batch.test.JobLauncherTestUtils] found for dependency: 
	expected at least 1 bean which qualifies as autowire candidate for this dependency. 
	......
Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:288)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1122)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireBeanProperties(AbstractAutowireCapableBeanFactory.java:379)
	at org.springframework.test.context.support.DependencyInjectionTestExecutionListener.injectDependencies(DependencyInjectionTestExecutionListener.java:110)
	at org.springframework.test.context.support.DependencyInjectionTestExecutionListener.prepareTestInstance(DependencyInjectionTestExecutionListener.java:75)
	at org.springframework.test.context.TestContextManager.prepareTestInstance(TestContextManager.java:313)
	... 
```

 ## 解决办法

将`spring-batch-test.jar`添加到类路径中不会自动创建`JobLauncherTestUtils` bean。

为了解决这个问题，在一个 Spring 配置文件中声明一个`JobLauncherTestUtils` bean。

spring/batch/config/test-context.xml

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd">

    <bean class="org.springframework.batch.test.JobLauncherTestUtils"/>

</beans> 
```

并将它加载到单元测试中。

```java
 @RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {
    "classpath:spring/batch/jobs/job-abc.xml",
    "classpath:spring/batch/config/context.xml",
    "classpath:spring/batch/config/test-context.xml"})
public class AppTest {

    @Autowired
    private JobLauncherTestUtils jobLauncherTestUtils;

    @Test
    public void launchJob() throws Exception {

        JobExecution jobExecution = jobLauncherTestUtils.launchJob();
        assertEquals(BatchStatus.COMPLETED, jobExecution.getStatus());

    }
} 
```

 ## 参考

1.  [Spring 批处理单元测试示例–jUnit 和 TestNG](http://web.archive.org/web/20190223065023/http://www.mkyong.com/spring-batch/spring-batch-unit-test-example/)
2.  [春季批量单元测试官方指南](http://web.archive.org/web/20190223065023/http://static.springsource.org/spring-batch/reference/html/testing.html)

[spring batch](http://web.archive.org/web/20190223065023/http://www.mkyong.com/tag/spring-batch/) [unit test](http://web.archive.org/web/20190223065023/http://www.mkyong.com/tag/unit-test/)







