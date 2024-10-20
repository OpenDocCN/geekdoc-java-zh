# 在 BeanExpressionContext 类型的对象上找不到 jobParameters

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-batch/jobparameters-cannot-be-found-on-object-of-type-beanexpressioncontext/>

创建一个简单的 Spring 批处理作业，将数据写入 csv 文件。csv 文件名取决于作业参数的传递，由 Spring EL 解释。

job-sample.xml

```java
 <bean id="csvFileItemWriter" class="org.springframework.batch.item.file.FlatFileItemWriter">

  <!-- write to this csv file -->
  <property name="resource" 
      value="file:outputs/csv/domain.done.#{jobParameters['pid']}.csv" />

  <property name="appendAllowed" value="false" />
  <property name="lineAggregator">
      <bean
	class="org.springframework.batch.item.file.transform.DelimitedLineAggregator">
	<property name="delimiter" value="," />
	<property name="fieldExtractor">
	    <bean class="org.springframework.batch.item.file.transform.BeanWrapperFieldExtractor">
		<property name="names" value="id, domainName" />
	    </bean>
	</property>
      </bean>
  </property>
</bean> 
```

作业上方的单元测试:

```java
 public class TestSampleJob extends AbstractTestNGSpringContextTests {

    @Autowired
    private JobLauncherTestUtils jobLauncherTestUtils;

    @Test
    public void launchJob() throws Exception {

    	JobParameters jobParameters = 
    	    new JobParametersBuilder().addString("pid", "10").toJobParameters();

        JobExecution jobExecution = jobLauncherTestUtils.launchJob(jobParameters);
        Assert.assertEquals(jobExecution.getStatus(), BatchStatus.COMPLETED);

    }
} 
```

## 问题

它会提示“找不到作业参数”错误消息:

```java
 Caused by: org.springframework.expression.spel.SpelEvaluationException: 
	EL1008E:(pos 0): Field or property 'jobParameters' cannot be found on object 
	of type 'org.springframework.beans.factory.config.BeanExpressionContext'
	at org.springframework.expression.spel.ast.PropertyOrFieldReference.readProperty(PropertyOrFieldReference.java:208)
	at org.springframework.expression.spel.ast.PropertyOrFieldReference.getValueInternal(PropertyOrFieldReference.java:72)
	at org.springframework.expression.spel.ast.CompoundExpression.getValueInternal(CompoundExpression.java:52) 
```

 ## 解决办法

在“步骤”开始之前，`jobParameters` bean 实际上不能被实例化。要修复它，需要使用范围为“Step”的后期绑定。

job-sample.xml

```java
 <bean id="csvFileItemWriter" 
	class="org.springframework.batch.item.file.FlatFileItemWriter" scope="step">
	<!-- ...... -->
  </bean> 
```

 ## 参考

1.  [春季批量-步骤范围](http://web.archive.org/web/20190201025040/http://static.springsource.org/spring-batch/reference/html/configureStep.html#step-scope)

[spring batch](http://web.archive.org/web/20190201025040/http://www.mkyong.com/tag/spring-batch/)







