# Java——获取正在运行的 JAR 文件的名称或路径

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-get-the-name-or-path-of-a-running-jar-file/>

在 Java 中，我们可以使用下面的代码片段来获取正在运行的 JAR 文件的路径。

```java
 // static
  String jarPath = ClassName.class
          .getProtectionDomain()
          .getCodeSource()
          .getLocation()
          .toURI()
          .getPath();

  // non-static
  String jarPath = getClass()
          .getProtectionDomain()
          .getCodeSource()
          .getLocation()
          .toURI()
          .getPath(); 
```

样本输出。

Terminal

```java
 /home/mkyong/projects/core-java/java-io/target/java-io.jar 
```

## 1.获取运行 JAR 的路径

1.1 创建一个可执行 JAR 文件。

pom.xml

```java
 <!-- Make this jar executable -->
  <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-jar-plugin</artifactId>
      <version>3.2.0</version>
      <configuration>
          <archive>
              <manifest>
                  <addClasspath>true</addClasspath>
                  <mainClass>com.mkyong.io.howto.resources.TestApp</mainClass>
              </manifest>
          </archive>
      </configuration>
  </plugin> 
```

1.2 运行下面的代码，以获取正在运行的 JAR 文件的名称或路径。

TestApp.java

```java
 package com.mkyong.io.howto.resources;

import java.net.URISyntaxException;

public class TestApp {

    public static void main(String[] args) {

        TestApp obj = new TestApp();

        try {

            // Get path of the JAR file
            String jarPath = TestApp.class
                    .getProtectionDomain()
                    .getCodeSource()
                    .getLocation()
                    .toURI()
                    .getPath();
            System.out.println("JAR Path : " + jarPath);

            // Get name of the JAR file
            String jarName = jarPath.substring(jarPath.lastIndexOf("/") + 1);
            System.out.printf("JAR Name: " + jarName);

        } catch (URISyntaxException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

Terminal

```java
 $ mvn clean package

$ java -jar target/java-io.jar

JAR Path : /home/mkyong/projects/core-java/java-io/target/java-io.jar
JAR Name: java-io.jar 
```

## 2.toURI()？

如果文件名或文件路径包含特殊字符，例如`%`，`.getLocation.getPath()`将对特殊字符进行编码。

```java
 try {

      // return raw, no encode
      String jarPath = TestApp.class
              .getProtectionDomain()
              .getCodeSource()
              .getLocation()
              .toURI()
              .getPath();
      System.out.println("JAR Path : " + jarPath);

      // url encoded
      String jarPath2 = TestApp.class
              .getProtectionDomain()
              .getCodeSource()
              .getLocation() //.toURI
              .getPath();
      System.out.println("JAR Path 2 : " + jarPath2);

  } catch (URISyntaxException e) {
      e.printStackTrace();
  } 
```

输出

Terminal

```java
 $ java -jar java-io%test.jar

JAR Path : /home/mkyong/projects/core-java/java-io/target/java-io%test.jar

JAR Path 2 : /home/mkyong/projects/core-java/java-io/target/java-io%25test.jar 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160046/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [代码源 JavaDoc](http://web.archive.org/web/20220629160046/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/security/CodeSource.html)
*   [RFC 2396–URI 的语法和格式](http://web.archive.org/web/20220629160046/https://www.ietf.org/rfc/rfc2396.txt)
*   [Maven–创建 JAR 文件](/web/20220629160046/https://mkyong.com/maven/how-to-create-a-jar-file-with-maven/)

<input type="hidden" id="mkyong-current-postId" value="16153">