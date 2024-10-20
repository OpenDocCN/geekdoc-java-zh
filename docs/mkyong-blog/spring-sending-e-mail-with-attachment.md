# spring–发送带附件的电子邮件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-sending-e-mail-with-attachment/>

这里有一个使用 Spring 通过 Gmail SMTP 服务器发送带有附件的电子邮件的例子。为了在你的电子邮件中包含附件，你必须使用 Spring 的**Java mail sender&mime message**，而不是**mail sender&simple mail message**。

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

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 2.春天的邮件发送者

你要用 **JavaMailSender** 代替 MailSender 发送附件，用 **MimeMessageHelper** 附加资源。在本例中，它将从您的文件系统(FileSystemResource)中获取“c:\\log.txt”文本文件作为电子邮件附件。

除了文件系统，你还可以从 URL path( **UrlResource** )、class path(**class path resource**)、InputStream(**InputStream resource**)……请参考 Spring 的 **AbstractResource** 实现类。

*文件:MailMail.java*

```java
 package com.mkyong.common;

import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;

import org.springframework.core.io.FileSystemResource;
import org.springframework.mail.MailParseException;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;

public class MailMail
{
	private JavaMailSender mailSender;
	private SimpleMailMessage simpleMailMessage;

	public void setSimpleMailMessage(SimpleMailMessage simpleMailMessage) {
		this.simpleMailMessage = simpleMailMessage;
	}

	public void setMailSender(JavaMailSender mailSender) {
		this.mailSender = mailSender;
	}

	public void sendMail(String dear, String content) {

	   MimeMessage message = mailSender.createMimeMessage();

	   try{
		MimeMessageHelper helper = new MimeMessageHelper(message, true);

		helper.setFrom(simpleMailMessage.getFrom());
		helper.setTo(simpleMailMessage.getTo());
		helper.setSubject(simpleMailMessage.getSubject());
		helper.setText(String.format(
			simpleMailMessage.getText(), dear, content));

		FileSystemResource file = new FileSystemResource("C:\\log.txt");
		helper.addAttachment(file.getFilename(), file);

	     }catch (MessagingException e) {
		throw new MailParseException(e);
	     }
	     mailSender.send(message);
         }
} 
```

## 3.Bean 配置文件

配置 mailSender bean、电子邮件模板，并指定 Gmail SMTP 服务器的电子邮件详细信息。

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

 Attachment : log.txt 
```

## 下载源代码

Download it – [Spring-Email-Attachment-Example.zip](http://web.archive.org/web/20210224142829/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-Email-Attachment-Example.zip)Tags : [email](http://web.archive.org/web/20210224142829/https://mkyong.com/tag/email/) [spring](http://web.archive.org/web/20210224142829/https://mkyong.com/tag/spring/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="4088">