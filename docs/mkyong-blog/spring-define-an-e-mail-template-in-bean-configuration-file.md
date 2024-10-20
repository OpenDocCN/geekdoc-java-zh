# spring——在 bean 配置文件中定义电子邮件模板

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-define-an-e-mail-template-in-bean-configuration-file/>

在上一个 [Spring 的电子邮件教程](http://web.archive.org/web/20190210101654/http://www.mkyong.com/spring/spring-sending-e-mail-via-gmail-smtp-server-with-mailsender/)中，你在方法体中硬编码了所有的电子邮件属性和消息内容，这是不实际的，应该避免。您应该考虑在 Spring 的 bean 配置文件中定义电子邮件消息模板。

## 1.项目依赖性

添加 JavaMail 和 Spring 的依赖项。

*文件:pom.xml*

```java
 <project  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
  http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mkyong.common</groupId>
  <artifactId>SpringExample</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>SpringExample</name>
  <url>http://maven.apache.org</url>

  <repositories>
  	<repository>
  		<id>Java.Net</id>
  		<url>http://download.java.net/maven/2/</url>
  	</repository>
  </repositories>

  <dependencies>

    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>3.8.1</version>
        <scope>test</scope>
    </dependency>

    <!-- Java Mail API -->
    <dependency>
	    <groupId>javax.mail</groupId>
	    <artifactId>mail</artifactId>
	    <version>1.4.3</version>
    </dependency>

    <!-- Spring framework -->
    <dependency>
     	<groupId>org.springframework</groupId>
	    <artifactId>spring</artifactId>
	    <version>2.5.6</version>
    </dependency>

  </dependencies>
</project> 
```

 ## 2.春天的邮件发送者

一个 Java 类，通过 Spring 的 MailSender 接口发送电子邮件，并使用 **String.format** 用 bean 配置文件中的传递变量替换电子邮件消息“ **%s** ”。

*文件:MailMail.java*

```java
 package com.mkyong.common;

import org.springframework.mail.MailSender;
import org.springframework.mail.SimpleMailMessage;

public class MailMail
{
	private MailSender mailSender;
	private SimpleMailMessage simpleMailMessage;

	public void setSimpleMailMessage(SimpleMailMessage simpleMailMessage) {
		this.simpleMailMessage = simpleMailMessage;
	}

	public void setMailSender(MailSender mailSender) {
		this.mailSender = mailSender;
	}

	public void sendMail(String dear, String content) {

	   SimpleMailMessage message = new SimpleMailMessage(simpleMailMessage);

	   message.setText(String.format(
			simpleMailMessage.getText(), dear, content));

	   mailSender.send(message);

	}	
} 
```

 ## 3.Bean 配置文件

在 bean 配置文件中定义电子邮件模板' **customeMailMessage** '和邮件发送者详细信息。

*文件:Spring-Mail.xml*

```java
 <beans 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

<bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl">
	<property name="host" value="smtp.gmail.com" />
	<property name="port" value="587" />
	<property name="username" value="username" />
	<property name="password" value="password" />

	<property name="javaMailProperties">
	     <props>
           	<prop key="mail.smtp.auth">true</prop>
           	<prop key="mail.smtp.starttls.enable">true</prop>
       	     </props>
	</property>
</bean>

<bean id="mailMail" class="com.mkyong.common.MailMail">
	<property name="mailSender" ref="mailSender" />
	<property name="simpleMailMessage" ref="customeMailMessage" />
</bean>

<bean id="customeMailMessage"
	class="org.springframework.mail.SimpleMailMessage">

	<property name="from" value="from@no-spam.com" />
	<property name="to" value="to@no-spam.com" />
	<property name="subject" value="Testing Subject" />
	<property name="text">
	   <value>
		<![CDATA[
			Dear %s,
			Mail Content : %s
		]]>
	   </value>
        </property>
</bean>

</beans> 
```

## 4.运行它

```java
 package com.mkyong.common;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
    	ApplicationContext context = 
           new ClassPathXmlApplicationContext("Spring-Mail.xml");

    	MailMail mm = (MailMail) context.getBean("mailMail");
        mm.sendMail("Yong Mook Kim", "This is text content");

    }
} 
```

输出

```java
 Dear Yong Mook Kim,
 Mail Content : This is text content 
```

## 下载源代码

Download it – [Spring-Email-Gmail-Smtp-Template-Example.zip](http://web.archive.org/web/20190210101654/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Email-Gmail-Smtp-Template-Example.zip)[email](http://web.archive.org/web/20190210101654/http://www.mkyong.com/tag/email/) [spring](http://web.archive.org/web/20190210101654/http://www.mkyong.com/tag/spring/)







