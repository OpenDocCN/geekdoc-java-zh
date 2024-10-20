# Spring Boot-基于配置文件的属性和 yaml 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-profile-based-properties-and-yaml-example/>

在 Spring Boot，它按以下顺序挑选`.properties`或`.yaml`文件:

*   `application-{profile}.properties`和 YAML 的变种
*   `application.properties`和 YAML 的变种

**Note**
For detail, sequence and order, please refer to this official [Externalized Configuration](http://web.archive.org/web/20220815221143/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) documentation.

测试:

*   Spring Boot 2.1.2 .版本
*   maven3

## 1.项目结构

一个标准的 Maven 项目结构。

![profile properties](img/16477de93b66f742f48ebb49bc541a50.png)![profile yaml](img/89466e7c31e9ca850657425848f15114.png)

## 2.@配置属性

稍后读取属性或 yaml 文件。

ServerProperties.java

```java
 package com.mkyong.config;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
@ConfigurationProperties("server")
public class ServerProperties {

    private String email;
    private List<Cluster> cluster = new ArrayList<>();

    public static class Cluster {
        private String ip;
        private String path;

        public String getIp() {
            return ip;
        }

        public void setIp(String ip) {
            this.ip = ip;
        }

        public String getPath() {
            return path;
        }

        public void setPath(String path) {
            this.path = path;
        }

        @Override
        public String toString() {
            return "Cluster{" +
                    "ip='" + ip + '\'' +
                    ", path='" + path + '\'' +
                    '}';
        }
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public List<Cluster> getCluster() {
        return cluster;
    }

    public void setCluster(List<Cluster> cluster) {
        this.cluster = cluster;
    }

    @Override
    public String toString() {
        return "ServerProperties{" +
                "email='" + email + '\'' +
                ", cluster=" + cluster +
                '}';
    }
} 
```

## 3.基于配置文件的属性

多个配置文件`.properties`示例。

application.properties

```java
 #Logging
logging.level.org.springframework.web=ERROR
logging.level.com.mkyong=ERROR
logging.level.=error

#spring
spring.main.banner-mode=off
spring.profiles.active=dev 
```

application-dev.properties

```java
 #dev environment
server.email: dev@mkyong.com
server.cluster[0].ip=127.0.0.1
server.cluster[0].path=/dev1
server.cluster[1].ip=127.0.0.2
server.cluster[1].path=/dev2
server.cluster[2].ip=127.0.0.3
server.cluster[2].path=/dev3 
```

application-prod.properties

```java
 #production environment
server.email: prod@mkyong.com
server.cluster[0].ip=192.168.0.1
server.cluster[0].path=/app1
server.cluster[1].ip=192.168.0.2
server.cluster[1].path=/app2
server.cluster[2].ip=192.168.0.3
server.cluster[2].path=/app3 
```

## 4.基于配置文件的 YAML

多个配置文件`.yml`示例。在 YAML，我们可以通过使用“—”分隔符来创建多个概要文件。

application.yml

```java
 logging:
  level:
    .: error
    org.springframework: ERROR
    com.mkyong: ERROR

spring:
  profiles:
    active: "dev"
  main:
    banner-mode: "off"

server:
  email: default@mkyong.com

---

spring:
  profiles: dev
server:
  email: dev@mkyong.com
  cluster:
    - ip: 127.0.0.1
      path: /dev1
    - ip: 127.0.0.2
      path: /dev2
    - ip: 127.0.0.3
      path: /dev3

---

spring:
  profiles: prod
server:
  email: prod@mkyong.com
  cluster:
    - ip: 192.168.0.1
      path: /app1
    - ip: 192.168.0.2
      path: /app2
    - ip: 192.168.0.3
      path: /app3 
```

## 5.演示

5.1 Spring Boot 应用程序。

Application.java

```java
 package com.mkyong;

import com.mkyong.config.ServerProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application implements CommandLineRunner {

    @Autowired
    private ServerProperties serverProperties;

    @Override
    public void run(String... args) {
        System.out.println(serverProperties);
    }

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

} 
```

5.2 打包并运行它。

```java
 $ mvn package

# The 'dev' profile is configured in application.properties

# Profile : dev , picks application-dev.properties or YAML 
$ java -jar target/spring-boot-profile-1.0.jar

ServerProperties{email='dev@mkyong.com', cluster=[
	Cluster{ip='127.0.0.1', path='/dev1'}, 
	Cluster{ip='127.0.0.2', path='/dev2'}, 
	Cluster{ip='127.0.0.3', path='/dev3'}
]}

# Profile : prod, picks application-prod.properties or YAML
$ java -jar -Dspring.profiles.active=prod target/spring-boot-profile-1.0.jar

ServerProperties{email='prod@mkyong.com', cluster=[
	Cluster{ip='192.168.0.1', path='/app1'}, 
	Cluster{ip='192.168.0.2', path='/app2'}, 
	Cluster{ip='192.168.0.3', path='/app3'}
]}

# Profile : abc, a non-exists profile
$ java -jar -Dspring.profiles.active=abc target/spring-boot-profile-1.0.jar
ServerProperties{email='null', cluster=[]} 
```

**Note**
Spring Boot, the default profile is `default`, we can set the profile via `spring.profiles.active` property.

## 下载源代码

$ git clone [https://github.com/mkyong/spring-boot.git](http://web.archive.org/web/20220815221143/https://github.com/mkyong/spring-boot.git)

*属性示例*
$ cd 配置文件-属性
$ mvn 包

*YAML 示例*
$ cd 简介-yaml
$mvn 包

## 参考

*   [Spring Boot–属性&配置](http://web.archive.org/web/20220815221143/https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html)
*   [Spring Boot–个人资料特定属性](http://web.archive.org/web/20220815221143/https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties)
*   [Spring Boot 简介示例](/web/20220815221143/https://mkyong.com/spring-boot/spring-boot-profiles-example/)
*   [弹簧轮廓示例](/web/20220815221143/https://mkyong.com/spring/spring-profiles-example/)
*   [Spring Boot 简介示例](/web/20220815221143/https://mkyong.com/spring-boot/spring-boot-profiles-example/)

<input type="hidden" id="mkyong-current-postId" value="14496">