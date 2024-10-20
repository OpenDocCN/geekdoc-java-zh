# spring–使用 MailSender 通过 Gmail SMTP 服务器发送电子邮件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-sending-e-mail-via-gmail-smtp-server-with-mailsender/>

Spring 附带了一个有用的'**org . spring framework . mail . JavaMail . javamailsenderimpl**'类来简化通过 JavaMail API 发送电子邮件的过程。这里有一个 Maven 构建项目，使用 Spring 的“ **JavaMailSenderImpl** 通过 Gmail SMTP 服务器发送电子邮件。

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

用 Spring 的 MailSender 接口发送电子邮件的 Java 类。

*文件:MailMail.java*

```java
 package com.mkyong.common;

import org.springframework.mail.MailSender;
import org.springframework.mail.SimpleMailMessage;

public class MailMail
{
	private MailSender mailSender;

	public void setMailSender(MailSender mailSender) {
		this.mailSender = mailSender;
	}

	public void sendMail(String from, String to, String subject, String msg) {

		SimpleMailMessage message = new SimpleMailMessage();

		message.setFrom(from);
		message.setTo(to);
		message.setSubject(subject);
		message.setText(msg);
		mailSender.send(message);	
	}
} 
```

## 3.Bean 配置文件

配置 mailSender bean 并指定 Gmail SMTP 服务器的电子邮件详细信息。

**Note**
Gmail configuration details – [http://mail.google.com/support/bin/answer.py?hl=en&answer=13287](http://web.archive.org/web/20200616054438/http://mail.google.com/support/bin/answer.py?hl=en&answer=13287)

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
        mm.sendMail("from@no-spam.com",
    		   "to@no-spam.com",
    		   "Testing123", 
    		   "Testing only \n\n Hello Spring Email Sender");

    }
} 
```

## 下载源代码

Download it – [Spring-Email-Gmail-Smtp-Example.zip](http://web.archive.org/web/20200616054438/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Email-Gmail-Smtp-Example.zip)Tags : [email](http://web.archive.org/web/20200616054438/https://mkyong.com/tag/email/) [gmail](http://web.archive.org/web/20200616054438/https://mkyong.com/tag/gmail/) [smtp](http://web.archive.org/web/20200616054438/https://mkyong.com/tag/smtp/) [spring](http://web.archive.org/web/20200616054438/https://mkyong.com/tag/spring/)<input type="hidden" id="mkyong-current-postId" value="4067">