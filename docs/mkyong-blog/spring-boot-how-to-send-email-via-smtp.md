# Spring Boot——如何通过 SMTP 发送电子邮件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-how-to-send-email-via-smtp/>

![spring boot sending email](img/c82bff568d36f10769fdb01f34955caf.png)

在本教程中，我们将向您展示如何在 Spring Boot 通过 SMTP 发送电子邮件。

使用的技术:

*   Spring Boot 2.1.2 .版本
*   弹簧 5.1.4 释放
*   Java 邮件 1.6.2
*   maven3
*   Java 8

## 1.项目目录

![project directory](img/cc9ef44bb29a3f12e23f2d3bbdffd827.png)

## 2.专家

`spring-boot-starter-mail`声明，为了发送电子邮件，它将提取 JavaMail 依赖项。

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>spring-boot-send-email</artifactId>
    <packaging>jar</packaging>
    <name>Spring Boot Send Email</name>
    <url>https://www.mkyong.com</url>
    <version>1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.2.RELEASE</version>
    </parent>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <!-- send email -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <!-- Package as an executable jar/war -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.0</version>
            </plugin>

        </plugins>
    </build>
</project> 
```

显示项目依赖关系。

```java
 $ mvn dependency:tree

[INFO] org.springframework.boot:spring-boot-send-email:jar:1.0
[INFO] +- org.springframework.boot:spring-boot-starter:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot:jar:2.1.2.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-context:jar:5.1.4.RELEASE:compile
[INFO] |  |     +- org.springframework:spring-aop:jar:5.1.4.RELEASE:compile
[INFO] |  |     \- org.springframework:spring-expression:jar:5.1.4.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-autoconfigure:jar:2.1.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-logging:jar:2.1.2.RELEASE:compile
[INFO] |  |  +- ch.qos.logback:logback-classic:jar:1.2.3:compile
[INFO] |  |  |  +- ch.qos.logback:logback-core:jar:1.2.3:compile
[INFO] |  |  |  \- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO] |  |  +- org.apache.logging.log4j:log4j-to-slf4j:jar:2.11.1:compile
[INFO] |  |  |  \- org.apache.logging.log4j:log4j-api:jar:2.11.1:compile
[INFO] |  |  \- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
[INFO] |  +- javax.annotation:javax.annotation-api:jar:1.3.2:compile
[INFO] |  +- org.springframework:spring-core:jar:5.1.4.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-jcl:jar:5.1.4.RELEASE:compile
[INFO] |  \- org.yaml:snakeyaml:jar:1.23:runtime

[INFO] \- org.springframework.boot:spring-boot-starter-mail:jar:2.1.2.RELEASE:compile
[INFO]    +- org.springframework:spring-context-support:jar:5.1.4.RELEASE:compile
[INFO]    |  \- org.springframework:spring-beans:jar:5.1.4.RELEASE:compile
[INFO]    \- com.sun.mail:javax.mail:jar:1.6.2:compile
[INFO]       \- javax.activation:activation:jar:1.1:compile 
```

## 3.Gmail SMTP

本例使用 Gmail SMTP 服务器，通过 TLS(端口 587)和 SSL(端口 465)进行测试。阅读此 [Gmail SMTP](http://web.archive.org/web/20230101145141/https://support.google.com/mail/answer/7126229)

application.properties

```java
 spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=username
spring.mail.password=password

# Other properties
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.connectiontimeout=5000
spring.mail.properties.mail.smtp.timeout=5000
spring.mail.properties.mail.smtp.writetimeout=5000

# TLS , port 587
spring.mail.properties.mail.smtp.starttls.enable=true

# SSL, post 465
#spring.mail.properties.mail.smtp.socketFactory.port = 465
#spring.mail.properties.mail.smtp.socketFactory.class = javax.net.ssl.SSLSocketFactory 
```

**Note**
For Gmail SMTP, if your account is enabled the 2-Step verification, please create an “App Password”. Read more on this [JavaMail – Sending email via Gmail SMTP example](http://web.archive.org/web/20230101145141/https://www.mkyong.com/java/javamail-api-sending-email-via-gmail-smtp-example/)

## 4.发送电子邮件

Spring 在[JavaMail](http://web.archive.org/web/20230101145141/https://javaee.github.io/javamail/)API 之上提供了一个`JavaMailSender`接口。

4.1 发送普通文本邮件。

```java
 import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;

	@Autowired
    private JavaMailSender javaMailSender;

	void sendEmail() {

        SimpleMailMessage msg = new SimpleMailMessage();
        msg.setTo("to_1@gmail.com", "to_2@gmail.com", "to_3@yahoo.com");

        msg.setSubject("Testing from Spring Boot");
        msg.setText("Hello World \n Spring Boot Email");

        javaMailSender.send(msg);

    } 
```

4.2 发送一封 HTML 邮件和一个文件附件。

```java
 import org.springframework.core.io.ClassPathResource;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;

import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;

	void sendEmailWithAttachment() throws MessagingException, IOException {

        MimeMessage msg = javaMailSender.createMimeMessage();

        // true = multipart message
        MimeMessageHelper helper = new MimeMessageHelper(msg, true);

        helper.setTo("to_@email");

        helper.setSubject("Testing from Spring Boot");

        // default = text/plain
        //helper.setText("Check attachment for image!");

        // true = text/html
        helper.setText("<h1>Check attachment for image!</h1>", true);

		// hard coded a file path
        //FileSystemResource file = new FileSystemResource(new File("path/android.png"));

        helper.addAttachment("my_photo.png", new ClassPathResource("android.png"));

        javaMailSender.send(msg);

    } 
```

## 5.演示

5.1 启动 Spring Boot，它会发出一封邮件。

Application.java

```java
 package com.mkyong;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.core.io.ClassPathResource;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;

import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;
import java.io.IOException;

@SpringBootApplication
public class Application implements CommandLineRunner {

    @Autowired
    private JavaMailSender javaMailSender;

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Override
    public void run(String... args) {

        System.out.println("Sending Email...");

        try {

            sendEmail();
            //sendEmailWithAttachment();

        } catch (MessagingException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("Done");

    }

    void sendEmail() {

        SimpleMailMessage msg = new SimpleMailMessage();
        msg.setTo("1@gmail.com", "2@yahoo.com");

        msg.setSubject("Testing from Spring Boot");
        msg.setText("Hello World \n Spring Boot Email");

        javaMailSender.send(msg);

    }

    void sendEmailWithAttachment() throws MessagingException, IOException {

        MimeMessage msg = javaMailSender.createMimeMessage();

        // true = multipart message
        MimeMessageHelper helper = new MimeMessageHelper(msg, true);
        helper.setTo("1@gmail.com");

        helper.setSubject("Testing from Spring Boot");

        // default = text/plain
        //helper.setText("Check attachment for image!");

        // true = text/html
        helper.setText("<h1>Check attachment for image!</h1>", true);

        helper.addAttachment("my_photo.png", new ClassPathResource("android.png"));

        javaMailSender.send(msg);

    }
} 
```

Terminal

```java
 .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.2.RELEASE)

Sending Email...
Done 
```

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20230101145141/https://github.com/mkyong/spring-boot.git)
$ cd email

//更新 application.properties 中的 SMTP 信息

$ mvn 弹簧启动:运行

## 参考

*   [春季邮件](http://web.archive.org/web/20230101145141/https://docs.spring.io/spring/docs/5.1.6.RELEASE/spring-framework-reference/integration.html#mail)
*   [Spring Boot 发送邮件](http://web.archive.org/web/20230101145141/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-email.html)
*   [Java 邮件](http://web.archive.org/web/20230101145141/https://javaee.github.io/javamail/)
*   [JavaMail–通过 Gmail SMTP 发送电子邮件示例](http://web.archive.org/web/20230101145141/https://www.mkyong.com/java/javamail-api-sending-email-via-gmail-smtp-example/)

<input type="hidden" id="mkyong-current-postId" value="15037">