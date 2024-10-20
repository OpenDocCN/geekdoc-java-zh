# 如何在 BeanWrapperFieldSetMapper 中转换日期

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-batch/how-to-convert-date-in-beanwrapperfieldsetmapper/>

读取下面的 Spring 批处理作业，它从" *domain.csv* "中读取数据，并将其映射到一个域对象。

job-example.xml

```java
 <bean class="org.springframework.batch.item.file.FlatFileItemReader" >

  <property name="resource" value="file:outputs/csv/domain.csv" />
  <property name="lineMapper">
    <bean class="org.springframework.batch.item.file.mapping.DefaultLineMapper">

	<property name="lineTokenizer">
	  <bean
		class="org.springframework.batch.item.file.transform.DelimitedLineTokenizer">
		<property name="names" value="id, domainName, lastModifiedDate" />
	  </bean>
	</property>
	<property name="fieldSetMapper">
	  <bean
		class="org.springframework.batch.item.file.mapping.BeanWrapperFieldSetMapper">
		<property name="prototypeBeanName" value="domain" />
	  </bean>
	</property>

    </bean>
  </property>

</bean>

 <bean id="domain" class="com.mkyong.batch.Domain" scope="prototype" /> 
```

domain.csv

```java
 1,facebook.com,Mon Jul 15 16:32:21 MYT 2013
2,google.com,Mon Jul 15 16:32:21 MYT 2013
3,youtube.com,Mon Jul 15 16:32:21 MYT 2013
4,yahoo.com,Mon Jul 15 16:32:21 MYT 2013
5,amazon.com,Mon Jul 15 16:32:21 MYT 2013 
```

Domain.java

```java
 import java.util.Date;

public class DomainRanking {

	private int id;
	private String domainName;
	private Date lastModifiedDate;

	//...
} 
```

## 问题

问题是如何将字符串日期`Mon Jul 15 16:32:21 MYT 2013`映射/转换为`java.util.Date`？运行上述作业将提示以下错误消息:

```java
 Cannot convert value of type [java.lang.String] to required type [java.util.Date] 
        for property 'lastModifiedDate': 
	no matching editors or conversion strategy found 
```

 ## 解决办法

参考[BeanWrapperFieldSetMapper JavaDoc](http://web.archive.org/web/20190210094907/http://static.springsource.org/spring-batch/apidocs/org/springframework/batch/item/file/mapping/BeanWrapperFieldSetMapper.html):

> 要定制将字段集值转换为注入原型所需类型的方式，有几种选择。您可以通过 customEditors 属性直接注入 PropertyEditor 实例…

为了修复它，声明一个`CustomDateEditor`并通过`customEditors`属性注入到`BeanWrapperFieldSetMapper`中。

job-example.xml

```java
 <bean id="dateEditor" 
  class="org.springframework.beans.propertyeditors.CustomDateEditor">
  <constructor-arg>
	<bean class="java.text.SimpleDateFormat">
              <constructor-arg value="EEE MMM dd HH:mm:ss z yyyy" />
	</bean>
  </constructor-arg>
  <constructor-arg value="true" /> 
</bean>

<bean class="org.springframework.batch.item.file.FlatFileItemReader" >

  <property name="resource" value="file:outputs/csv/domain.csv" />
  <property name="lineMapper">
    <bean class="org.springframework.batch.item.file.mapping.DefaultLineMapper">
	<property name="lineTokenizer">
	  <bean
		class="org.springframework.batch.item.file.transform.DelimitedLineTokenizer">
		<property name="names" value="id, domainName, lastModifiedDate" />
	  </bean>
	</property>
	<property name="fieldSetMapper">
	  <bean
		class="org.springframework.batch.item.file.mapping.BeanWrapperFieldSetMapper">
		<property name="prototypeBeanName" value="domain" />
		<property name="customEditors">
		  <map>
			<entry key="java.util.Date">
			     <ref local="dateEditor" />
			</entry>
		  </map>
		</property>
	  </bean>
	</property>

    </bean>
  </property>
</bean>

<bean id="domain" class="com.mkyong.batch.Domain" scope="prototype" /> 
```

字符串日期“MYT 2013 年 7 月 15 日星期一 16:32:21”由“EEE MMM dd HH:mm:ss z yyyy”表示。

**Note**
Do not injects the `CustomDateEditor` via `CustomEditorConfigurer` (globally), it will not works.

```java
 <bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
	<property name="customEditors">
	  <map>
		<entry key="java.util.Date">
			<ref local="dateEditor" />
		</entry>
	  </map>
	</property>
    </bean> 
```

 ## 参考

1.  [BeanWrapperFieldSetMapper JavaDoc](http://web.archive.org/web/20190210094907/http://static.springsource.org/spring-batch/apidocs/org/springframework/batch/item/file/mapping/BeanWrapperFieldSetMapper.html)
2.  [BeanWrapperFieldSetMapper 不适用于日期](http://web.archive.org/web/20190210094907/http://forum.springsource.org/showthread.php?68551-BeanWrapperFieldSetMapper-not-working-for-Dates)
3.  [java.text.SimpleDateFormat JavaDoc](http://web.archive.org/web/20190210094907/http://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html)
4.  [Spring Batch–如何将字符串从文件转换为日期](http://web.archive.org/web/20190210094907/http://stackoverflow.com/questions/9059481/spring-batch-how-to-convert-string-from-file-to-date)
5.  [Spring 将日期注入 Bean 属性-custom Date editor](http://web.archive.org/web/20190210094907/http://www.mkyong.com/spring/spring-how-to-pass-a-date-into-bean-property-customdateeditor/)
6.  [如何将字符串转换为日期–Java](http://web.archive.org/web/20190210094907/http://www.mkyong.com/java/how-to-convert-string-to-date-java/)

[date conversion](http://web.archive.org/web/20190210094907/http://www.mkyong.com/tag/date-conversion/) [spring batch](http://web.archive.org/web/20190210094907/http://www.mkyong.com/tag/spring-batch/)







