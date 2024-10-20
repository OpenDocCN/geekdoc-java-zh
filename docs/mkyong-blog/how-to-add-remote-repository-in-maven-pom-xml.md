# 如何在 Maven 中添加远程存储库

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/how-to-add-remote-repository-in-maven-pom-xml/>

并不是每个库都存储在 [Maven Central Repository](http://web.archive.org/web/20220906133656/https://www.mkyong.com/maven/where-is-maven-central-repository/) 中，有些库只在 Java.net 或 JBoss repository(远程库)中可用。

## 1.Java.net 知识库

pom.xml

```java
 <repositories>
        <repository>
            <id>java-net-repo</id>
            <url>https://maven.java.net/content/repositories/public/</url>
        </repository>     
  </repositories> 
```

## 2.JBoss 仓库

pom.xml

```java
 <repositories>
        <repository>
            <id>jboss-repo</id>
            <url>http://repository.jboss.org/nexus/content/groups/public/</url>
        </repository>
  </repositories> 
```

## 3.Spring 知识库

pom.xml

```java
 <repositories>
        <repository>
            <id>spring-repo</id>
            <url>https://repo.spring.io/release</url>
        </repository>
  </repositories> 
```

## 参考

1.  [配置 Maven 使用 JBoss 存储库](http://web.archive.org/web/20220906133656/https://developer.jboss.org/wiki/MavenGettingStarted-Developers)
2.  [泉库](http://web.archive.org/web/20220906133656/https://docs.spring.io/spring-android/docs/2.0.0.M3/reference/html/maven.html)

<input type="hidden" id="mkyong-current-postId" value="2261">