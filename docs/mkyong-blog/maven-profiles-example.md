# Maven 配置文件示例

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/maven-profiles-example/>

在本文中，我们将向您展示几个 Maven profile 示例，为不同的环境(开发、测试或生产)传递不同的参数(服务器或数据库参数)。

*用 Maven 3.5.3 测试的 PS*

## 1.基本 Maven 配置文件

1.1 跳过单元测试的简单概要。

pom.xml

```java
 <!-- skip unit test -->
	<profile>
		<id>xtest</id>
		<properties>
			<maven.test.skip>true</maven.test.skip>
		</properties>
	</profile> 
```

1.2 要激活一个数据图表，添加`-P`选项。

Terminal

```java
 # Activate xtest profile to skip unit test and package the project

$ mvn package -Pxtest 
```

1.3 要激活多个配置文件:

Terminal

```java
 $ mvn package -P xtest, another-profile-id

# multi modules, same syntax
$ mvn -pl module-name package -P xtest, another-profile-id 
```

1.4 在编译或打包阶段，总是添加`maven-help-plugin`来显示活动的概要文件，这将为您节省大量调试时间。

pom.xml

```java
 <build>
        <plugins>
            <!-- display active profile in compile phase -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-help-plugin</artifactId>
                <version>3.1.0</version>
                <executions>
                    <execution>
                        <id>show-profiles</id>
                        <phase>compile</phase>
                        <goals>
                            <goal>active-profiles</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

        </plugins>
    </build> 
```

下一次，当前活动的概要文件将在编译阶段显示。

Terminal

```java
 $ mvn compile -P xtest

[INFO] --- maven-help-plugin:3.1.0:active-profiles (show-profiles) @ example1 ---
[INFO]
Active Profiles for Project 'com.mkyong:example1:jar:1.0':

The following profiles are active:

 - xtest (source: com.mkyong:example1:1.0) 
```

## 2.Maven 配置文件–示例 1

将不同的属性值传递给开发和生产环境的 Maven profile 示例。

![](img/30ecfc6d43845bc871bbe00925985cb2.png)

2.1 属性文件。

resources/db.properties

```java
 db.driverClassName=${db.driverClassName}
db.url=${db.url}
db.username=${db.username}
db.password=${db.password} 
```

2.2 启用过滤。Maven 将把`resources/db.properties`中的`${}`映射到活动的 Maven 配置文件属性。

**Note**
Read this [Maven filtering](http://web.archive.org/web/20221207131421/https://maven.apache.org/plugins/maven-resources-plugin/examples/filter.html)pom.xml

```java
 <!-- map ${} variable -->
	<resources>
		<resource>
			<directory>src/main/resources</directory>
			<filtering>true</filtering>
		</resource>
	</resources> 
```

2.3 创建两个具有不同属性值的配置文件 id(dev 和 prod)。

pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <parent>
        <artifactId>maven-profiles</artifactId>
        <groupId>com.mkyong</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>example1</artifactId>

    <profiles>

        <profile>
            <id>dev</id>
            <activation>
                <!-- this profile is active by default -->
                <activeByDefault>true</activeByDefault>
                <!-- activate if system properties 'env=dev' -->
                <property>
                    <name>env</name>
                    <value>dev</value>
                </property>
            </activation>
            <properties>
                <db.driverClassName>com.mysql.jdbc.Driver</db.driverClassName>
                <db.url>jdbc:mysql://localhost:3306/dev</db.url>
                <db.username>mkyong</db.username>
                <db.password>8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92</db.password>
            </properties>
        </profile>

        <profile>
            <id>prod</id>
            <activation>
                <!-- activate if system properties 'env=prod' -->
                <property>
                    <name>env</name>
                    <value>prod</value>
                </property>
            </activation>
            <properties>
                <db.driverClassName>com.mysql.jdbc.Driver</db.driverClassName>
                <db.url>jdbc:mysql://live01:3306/prod</db.url>
                <db.username>mkyong</db.username>
                <db.password>8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92</db.password>
            </properties>
        </profile>

    </profiles>

    <build>

        <!-- map ${} variable -->
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
        </resources>

        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.0</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                        implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>com.mkyong.example1.App1</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

        </plugins>
    </build>

</project> 
```

2.4 加载属性文件并将其打印出来。

App1.java

```java
 package com.mkyong.example1;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class App1 {

    public static void main(String[] args) {

        App1 app = new App1();
        Properties prop = app.loadPropertiesFile("db.properties");
        prop.forEach((k, v) -> System.out.println(k + ":" + v));

    }

    public Properties loadPropertiesFile(String filePath) {

        Properties prop = new Properties();

        try (InputStream resourceAsStream = getClass().getClassLoader().getResourceAsStream(filePath)) {
            prop.load(resourceAsStream);
        } catch (IOException e) {
            System.err.println("Unable to load properties file : " + filePath);
        }

        return prop;

    }
} 
```

2.5 测试一下。

Terminal

```java
 # default profile id is 'dev'
$ mvn package

$ java -jar target/example1-1.0.jar
db.password:8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
db.driverClassName:com.mysql.jdbc.Driver
db.username:mkyong
db.url:jdbc:mysql://localhost:3306/dev

# enable profile id 'prod' with -P prod or -D env=prod
$ mvn package -P prod
$ mvn package -D env=prod

$ java -jar target/example1-1.0.jar
db.password:8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
db.driverClassName:com.mysql.jdbc.Driver
db.username:mkyong
db.url:jdbc:mysql://live01:3306/prod 
```

## 3.Maven 配置文件–示例 2

这个 Maven 概要文件示例将把所有内容放在属性文件中。

![](img/929687869b6dfb12978848925a9a244c.png)

3.1 一个属性文件，稍后 Maven 将根据配置文件 id 映射该值。

resources/config.properties

```java
 # Database Config
db.driverClassName=${db.driverClassName}
db.url=${db.url}
db.username=${db.username}
db.password=${db.password}

# Email Server
email.server=${email.server}

# Log Files
log.file.location=${log.file.location} 
```

3.2 为开发、测试和生产环境创建不同的属性文件。

resources/env/config.dev.properties

```java
 # Database Config
db.driverClassName=com.mysql.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/dev
db.username=mkyong
db.password=8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92

# Email Server
email.server=email-dev:8888

# Log Files
log.file.location=dev/file.log 
```

resources/env/config.test.properties

```java
 # Database Config
db.driverClassName=com.mysql.jdbc.Driver
db.url=jdbc:mysql://test01:3306/test
db.username=mkyong
db.password=8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92

# Email Server
email.server=email-test:8888

# Log Files
log.file.location=test/file.log 
```

resources/env/config.prod.properties

```java
 # Database Config
db.driverClassName=com.mysql.jdbc.Driver
db.url=jdbc:mysql://live01:3306/prod
db.username=mkyong
db.password=8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92

# Email Server
email.server=email-prod:25

# Log Files
log.file.location=prod/file.log 
```

3.3 启用过滤。这是关键！

**Note**
Read this [Maven filtering](http://web.archive.org/web/20221207131421/https://maven.apache.org/plugins/maven-resources-plugin/examples/filter.html)pom.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<project 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <parent>
        <artifactId>maven-profiles</artifactId>
        <groupId>com.mkyong</groupId>
        <version>1.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>example2</artifactId>

    <profiles>

        <profile>
            <id>dev</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <env>dev</env>
            </properties>
        </profile>

        <profile>
            <id>prod</id>
            <properties>
                <env>prod</env>
            </properties>
        </profile>

        <profile>
            <id>test</id>
            <properties>
                <env>test</env>
            </properties>
        </profile>

    </profiles>

    <build>

        <!-- Loading all ${} -->
        <filters>
            <filter>src/main/resources/env/config.${env}.properties</filter>
        </filters>

        <!-- Map ${} into resources -->
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
                <includes>
                    <include>*.properties</include>
                </includes>
            </resource>
        </resources>

        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.0</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                        implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>com.mkyong.example2.App2</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>

    </build>

</project> 
```

3.4 加载属性文件并将其打印出来。

App1.java

```java
 package com.mkyong.example2;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class App2 {

    public static void main(String[] args) {

        App2 app = new App2();
        Properties prop = app.loadPropertiesFile("config.properties");
        prop.forEach((k, v) -> System.out.println(k + ":" + v));

    }

    public Properties loadPropertiesFile(String filePath) {

        Properties prop = new Properties();

        try (InputStream resourceAsStream = getClass().getClassLoader().getResourceAsStream(filePath)) {
            prop.load(resourceAsStream);
        } catch (IOException e) {
            System.err.println("Unable to load properties file : " + filePath);
        }

        return prop;

    }

} 
```

3.5 测试一下。

Terminal

```java
 # profile id dev (default) 
$ mvn package

$ java -jar target/example2-1.0.jar
log.file.location:dev/file.log
db.password:8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
db.driverClassName:com.mysql.jdbc.Driver
db.username:mkyong
email.server:email-dev:8888
db.url:jdbc:mysql://localhost:3306/dev

# profile id prod
$ mvn package -P prod

$ java -jar target/example2-1.0.jar
log.file.location:prod/file.log
db.password:8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
db.driverClassName:com.mysql.jdbc.Driver
db.username:mkyong
email.server:email-prod:25
db.url:jdbc:mysql://live01:3306/prod

# profile id test
$ mvn package -P test

$ java -jar target/example2-1.0.jar
log.file.location:test/file.log
db.password:8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
db.driverClassName:com.mysql.jdbc.Driver
db.username:mkyong
email.server:email-test:8888
db.url:jdbc:mysql://test01:3306/test 
```

最后，让我知道你的用例🙂

## 下载源代码

$ git clone [https://github.com/mkyong/maven-examples.git](http://web.archive.org/web/20221207131421/https://github.com/mkyong/maven-examples.git)
$ cd maven-profiles

#使用配置文件' prod '
$ mvn-pl example 1 package-Pprod
$ Java-jar example 1/target/example 1-1.0 . jar 测试示例 1

#使用配置文件“Test”
$ mvn-pl example 2 package-Ptest
$ Java-jar example 2/target/example 2-1.0 . jar 测试示例 2

## 参考

1.  [构建概要文件简介](http://web.archive.org/web/20221207131421/https://maven.apache.org/guides/introduction/introduction-to-profiles.html)
2.  [Maven 最佳实践](http://web.archive.org/web/20221207131421/https://blog.tallan.com/2010/09/16/maven-best-practices/)
3.  [使用 Maven 2 为不同环境构建](http://web.archive.org/web/20221207131421/https://maven.apache.org/guides/mini/guide-building-for-different-environments.html)
4.  [Maven 过滤](http://web.archive.org/web/20221207131421/https://maven.apache.org/plugins/maven-resources-plugin/examples/filter.html)

<input type="hidden" id="mkyong-current-postId" value="14772">