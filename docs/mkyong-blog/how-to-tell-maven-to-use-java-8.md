# 如何告诉 Maven 使用 Java 8

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/maven/how-to-tell-maven-to-use-java-8/>

在`pom.xml`中，定义了这个`maven.compiler.source`属性来告诉 Maven 使用 Java 8 来编译项目。

## 1.Maven 属性

Java 8

pom.xml

```java
 <properties>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.source>1.8</maven.compiler.source>
    </properties> 
```

Java 7

pom.xml

```java
 <properties>
        <maven.compiler.target>1.7</maven.compiler.target>
        <maven.compiler.source>1.7</maven.compiler.source>
    </properties> 
```

 ## 2.编译器插件

或者，直接配置插件。

pom.xml

```java
 <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build> 
```

 ## 参考

1.  [Maven–设置 Java 编译器的-source 和-target](http://web.archive.org/web/20190224170051/https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html)
2.  [如何用不同的 JDK 版本编译 Maven 项目？](http://web.archive.org/web/20190224170051/https://www.mkyong.com/maven/how-to-compile-maven-project-with-different-jdk-version/)

[compiler](http://web.archive.org/web/20190224170051/http://www.mkyong.com/tag/compiler/) [java 8](http://web.archive.org/web/20190224170051/http://www.mkyong.com/tag/java-8/) [maven](http://web.archive.org/web/20190224170051/http://www.mkyong.com/tag/maven/)







