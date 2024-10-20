# Spring Boot——从哪门主课开始

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-which-main-class-to-start/>

如果 Spring Boot 项目包含多个主类，Spring Boot 将无法启动或打包进行部署。

Terminal

```java
 $ mvn package 
#or
$ mvn spring-boot:run

Failed to execute goal org.springframework.boot:spring-boot-maven-plugin:1.4.2.RELEASE:run (default-cli) 
Execution default-cli of goal org.springframework.boot:spring-boot-maven-plugin:1.4.2.RELEASE:run failed: 
Unable to find a single main class from the following candidates 
 [com.mkyong.Test, com.mkyong.SpringBootWebApplication] -> [Help 1] 
```

## Maven 示例

1.1 通过`start-class`属性定义单个主类

pom.xml

```java
 <properties>
      <!-- The main class to start by executing java -jar -->
      <start-class>com.mkyong.SpringBootWebApplication</start-class>
  </properties> 
```

1.2 或者，在`spring-boot-maven-plugin`中定义主类

pom.xml

```java
 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.mkyong.SpringBootWebApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build> 
```

 ## 参考

1.  [Spring Boot——可执行的 jar 格式](http://web.archive.org/web/20190217051121/https://docs.spring.io/spring-boot/docs/current/reference/html/executable-jar.html)
2.  [Spring Boot Maven 插件–用途](http://web.archive.org/web/20190217051121/http://docs.spring.io/spring-boot/docs/current/maven-plugin/usage.html)

[executable jar](http://web.archive.org/web/20190217051121/http://www.mkyong.com/tag/executable-jar/) [jar](http://web.archive.org/web/20190217051121/http://www.mkyong.com/tag/jar/) [main class](http://web.archive.org/web/20190217051121/http://www.mkyong.com/tag/main-class/) [spring boot](http://web.archive.org/web/20190217051121/http://www.mkyong.com/tag/spring-boot/) [war](http://web.archive.org/web/20190217051121/http://www.mkyong.com/tag/war/)![](img/3e5b2f2bc8da8f255ef662f1acf8decb.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190217051121/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="14251">







